### brain_mask nii화 + 해당 subject의 DTI file 경로로 이동
```
export FREESURFER_HOME=/usr/local/freesurfer
source $FREESURFER_HOME/SetUpFreeSurfer.sh

#!/bin/bash

# target_path를 설정하세요
target_path="/"

# 현재 경로의 모든 디렉토리들을 반복
for dir in */; do
    # 현재 디렉토리의 이름에서 마지막 슬래시 제거
    dir_name=$(basename "$dir")

    # mri/brainmask.mgz 파일 경로
    brainmask_file="$dir/mri/brainmask.mgz"

    # 타겟 경로에 같은 이름의 디렉토리가 있는지 확인
    if [ -d "$target_path/$dir_name" ]; then
        # mri_convert 명령어로 mgz 파일을 nii.gz 형식으로 변환하고 복사
        mri_convert "$brainmask_file" "$target_path/${dir_name}_brainmask.nii.gz"
        echo "Converted and copied $brainmask_file to $target_path/${dir_name}_brainmask.nii.gz"
    else
        echo "Directory $target_path/$dir_name does not exist."
    fi
done
```

### brainmask 넣어주는 명령어
```
for subject in $(ls -d */); do
    subject_name=${subject%/} # subject 디렉토리 이름 추출
    nii_file=$(ls | grep "${subject_name}.*\.nii\.gz") # subject 이름이 포함된 .nii.gz 파일 찾기
    if [ -n "$nii_file" ]; then
        target_dir=$(find "${subject_name}" -type d -name "NIFTI") # NIFTI 디렉토리 찾기
        if [ -n "$target_dir" ]; then
            mv "$nii_file" "$target_dir/${subject_name}.nii.gz" # 파일을 NIFTI 디렉토리로 이동
            echo "Moved $nii_file to $target_dir"
        else
            echo "NIFTI directory not found for $subject_name"
        fi
    else
        echo "No .nii.gz file found for $subject_name"
    fi
done
```


```
###############################################################################################################################################################################
### bval 파일에서 0을 제외한 부분 추출하기
###############################################################################################################################################################################
### subject 한개
###############################################################################################################################################################################
# bval 파일에서 처음 값을 제외한 나머지 값들 중 0인 것들을 제거하는 명령어
awk '{for(i=1;i<=NF;i++) if(i==1 || $i!=0) printf $i" "; print ""}' before.bval > filtered.bval
```

```
###############################################################################################################################################################################
### subject 여러개
###############################################################################################################################################################################
# Loop through each subdirectory under the current directory
for dir in $(find . -type d -path '*/NIFTI'); do
    # Define the path for the bval file in the current directory
    bval_file="$dir/DICOM_Axial_DTI_*.bval"
    
    # Create a filtered bval file
    awk '{for(i=1;i<=NF;i++) if(i==1 || $i!=0) printf $i" "; print ""}' $bval_file > "$dir/filtered.bval"
done
```

```
###############################################################################################################################################################################
### bvec 파일에서 해당 인덱스에 해당하는 값 제거
###############################################################################################################################################################################
### subject 한개
###############################################################################################################################################################################
# bvec 파일에서 b-value가 0인 부분을 제외하는 명령어
# Get indices of b-values that are 0, excluding the first index
zero_indices=$(awk 'NR==1 {for(i=2;i<=NF;i++) if($i==0) printf(i-1)" "}' before.bval)

# Create a filtered bvec file
awk -v indices="$zero_indices" '
BEGIN {
    split(indices, index_arr);
}
{
    for (i = 1; i <= NF; i++) {
        # Check if the current index is in the list of indices to delete
        delete_col = 0;
        for (j in index_arr) {
            if (i == index_arr[j] + 1) {  # +1 because awk arrays are 1-based
                delete_col = 1;
                break;
            }
        }
        if (!delete_col) {
            printf "%s ", $i;
        }
    }
    print "";  # New line after each row
}' before.bvec > filtered.bvec
```

```
###############################################################################################################################################################################
### subject 여러개
###############################################################################################################################################################################
# Loop through each subdirectory under the current directory
for dir in $(find . -type d -path '*/NIFTI'); do
    # Define the paths for the bval and bvec files in the current directory
    bval_file="$dir/DICOM_Axial_DTI_*.bval"
    bvec_file="$dir/DICOM_Axial_DTI_*.bvec"
    
    # Get indices of b-values that are 0, excluding the first index
    zero_indices=$(awk 'NR==1 {for(i=2;i<=NF;i++) if($i==0) printf(i-1)" "}' $bval_file)

    # Create a filtered bvec file
    awk -v indices="$zero_indices" '
    BEGIN {
        split(indices, index_arr);
    }
    {
        for (i = 1; i <= NF; i++) {
            # Check if the current index is in the list of indices to delete
            delete_col = 0;
            for (j in index_arr) {
                if (i == index_arr[j] + 1) {  # +1 because awk arrays are 1-based
                    delete_col = 1;
                    break;
                }
            }
            if (!delete_col) {
                printf "%s ", $i;
            }
        }
        print "";  # New line after each row
    }' $bvec_file > "$dir/filtered.bvec"
done
```

```
###############################################################################################################################################################################
### NIfTI 파일에서 해당 인덱스의 slice 제거
###############################################################################################################################################################################
### subject 한개
###############################################################################################################################################################################
# Get indices of non-zero b-values excluding the first index
non_zero_indices=$(awk 'NR==1 {print 0} {for(i=2;i<=NF;i++) if($i!=0) printf(i-1)" "}' before.bval)

# Use fslroi to extract volumes based on non-zero indices
for index in $non_zero_indices; do
    fslroi before.nii.gz output_$(printf "%03d" $index).nii.gz $index 1
done

# Merge the resulting volumes back together
fslmerge -t output_filtered_0.nii.gz $(ls output_*.nii.gz | sort)
```

```
###############################################################################################################################################################################
### subject 여러개
###############################################################################################################################################################################
# Loop through each subdirectory under the current directory
for dir in $(find . -type d -path '*/NIFTI'); do
    # Define the paths for the bval and nii.gz files in the current directory
    bval_file="$dir/DICOM_Axial_*.bval"
    nii_file="$dir/DICOM_Axial_*.nii.gz"
    
    # Get indices of non-zero b-values excluding the first index
    non_zero_indices=$(awk 'NR==1 {print 0} {for(i=2;i<=NF;i++) if($i!=0) printf(i-1)" "}' $bval_file)

    # Use fslroi to extract volumes based on non-zero indices
    for index in $non_zero_indices; do
        fslroi $nii_file "$dir/output_$(printf "%03d" $index).nii.gz" $index 1
    done

    # Merge the resulting volumes back together
    fslmerge -t "$dir/output_filtered_0.nii.gz" $(find "$dir" -name 'output_*.nii.gz' | sort)
done
```
