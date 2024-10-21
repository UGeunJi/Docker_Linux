'''
1. 마스크 옮기기. 
/nasdata4/CNAlab_data/BDSP/ADNI/{site}/ADNI/IMAGE/BRAIN/MRI/derivative/*{sub}/ses-01/anat/CIVET/mask/civet_*{sub}_ses-01_T1w_type-1_derivative-brain_mask.nii.gz

2. T1 가져오기. 
/nasdata4/CNAlab_data/BDSP/ADNI/{site}/ADNI/IMAGE/BRAIN/MRI/derivative/*{sub}/ses-01/anat/CIVET/final/civet_*{sub}_ses-01_T1w_type-1_derivative-t1_final.nii.gz

3. 이름 바꾸기 
{sub}_dti.nii(.gz)
{sub}_dti.bval(s)
{sub}_dti.bvec(s)
{sub}_dti.json
{sub}_T1.nii(.gz)
'''

'''
########################################################################################################################################################################################################
########################################################################################################################################################################################################
########################################################################################################################################################################################################
'''

'''
# ADNI_GO
#!/bin/bash
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름
    # 하위 디렉토리 이름을 포함한 경로 찾기
    dest_path=$(find /nasdata4/5ug/Graph_data/ADNI_GO/DTI/ -type d -path "*/${sub_dir_name}/*/NIFTI" -print -quit)
    
    # find 명령어를 사용해 경로에 해당하는 파일 찾기
    source_file=$(find /nasdata4/CNAlab_data/BDSP/ADNI/GO/ADNI/IMAGE/BRAIN/MRI/derivative/ -name "civet_sub-*${sub_dir_name}_ses-01_T1w_type-1_derivative-brain_mask.nii.gz" -print -quit)

    # 목적지 경로와 소스 파일이 모두 존재하는지 확인
    if [ -n "$source_file" ] && [ -n "$dest_path" ]; then  
        cp "$source_file" "$dest_path"
        echo "Copied $source_file to $dest_path"
    else
        if [ -z "$source_file" ]; then
            echo "No nii.gz file found for $sub_dir_name"
        fi
        if [ -z "$dest_path" ]; then
            echo "No destination path found for $sub_dir_name"
        fi
    fi
done



# ADNI2
#!/bin/bash
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름
    # 하위 디렉토리 이름을 포함한 경로 찾기
    dest_path=$(find /nasdata4/5ug/Graph_data/ADNI2/DTI/ -type d -path "*/${sub_dir_name}/*/NIFTI" -print -quit)
    
    # find 명령어를 사용해 경로에 해당하는 파일 찾기
    source_file=$(find /nasdata4/CNAlab_data/BDSP/ADNI/2/ADNI/IMAGE/BRAIN/MRI/derivative/ -name "civet_sub-*${sub_dir_name}_ses-01_T1w_type-1_derivative-brain_mask.nii.gz" -print -quit)

    # 목적지 경로와 소스 파일이 모두 존재하는지 확인
    if [ -n "$source_file" ] && [ -n "$dest_path" ]; then  
        cp "$source_file" "$dest_path"
        echo "Copied $source_file to $dest_path"
    else
        if [ -z "$source_file" ]; then
            echo "No nii.gz file found for $sub_dir_name"
        fi
        if [ -z "$dest_path" ]; then
            echo "No destination path found for $sub_dir_name"
        fi
    fi
done



# ADNI3
#!/bin/bash
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름
    # 하위 디렉토리 이름을 포함한 경로 찾기
    dest_path=$(find /nasdata4/5ug/Graph_data/ADNI3/DTI/ -type d -path "*/${sub_dir_name}/*/NIFTI" -print -quit)
    
    # find 명령어를 사용해 경로에 해당하는 파일 찾기
    source_file=$(find /nasdata4/CNAlab_data/BDSP/ADNI/3/ADNI/IMAGE/BRAIN/MRI/derivative/ -name "civet_sub-*${sub_dir_name}_ses-01_T1w_type-1_derivative-brain_mask.nii.gz" -print -quit)

    # 목적지 경로와 소스 파일이 모두 존재하는지 확인
    if [ -n "$source_file" ] && [ -n "$dest_path" ]; then  
        cp "$source_file" "$dest_path"
        echo "Copied $source_file to $dest_path"
    else
        if [ -z "$source_file" ]; then
            echo "No nii.gz file found for $sub_dir_name"
        fi
        if [ -z "$dest_path" ]; then
            echo "No destination path found for $sub_dir_name"
        fi
    fi
done
'''

'''
########################################################################################################################################################################################################
########################################################################################################################################################################################################
########################################################################################################################################################################################################
'''

'''
# ADNI_GO
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름
    
    # 경로에서 '*' 사용을 위해 find 명령어에 -path 옵션과 -type d를 사용
    dest_path=$(find /nasdata4/5ug/Graph_data/ADNI_GO/DTI/ -path "*/${sub_dir_name}/*/NIFTI" -type d | head -n 1)
    
    # source_file 경로에서도 동일하게 find 명령어를 사용
    source_file=$(find /nasdata4/CNAlab_data/BDSP/ADNI/GO/ADNI/IMAGE/BRAIN/MRI/derivative/ -path "*/${sub_dir_name}/ses-01/anat/CIVET/final/civet_sub-*${sub_dir_name}_ses-01_T1w_type-1_derivative-t1_final.nii.gz" -type f | head -n 1)

    if [ -n "$source_file" ] && [ -n "$dest_path" ]; then  
        cp "$source_file" "$dest_path"
        echo "Copied $source_file to $dest_path"
    else
        if [ -z "$source_file" ]; then
            echo "No nii.gz file found for $sub_dir_name"
        fi
        if [ -z "$dest_path" ]; then
            echo "No destination path found for $sub_dir_name"
        fi
    fi
done


# ADNI2
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름
    
    # 경로에서 '*' 사용을 위해 find 명령어에 -path 옵션과 -type d를 사용
    dest_path=$(find /nasdata4/5ug/Graph_data/ADNI2/temp3/ -path "*/${sub_dir_name}/*/NIFTI" -type d | head -n 1)
    
    # source_file 경로에서도 동일하게 find 명령어를 사용
    source_file=$(find /nasdata4/CNAlab_data/BDSP/ADNI/2/ADNI/IMAGE/BRAIN/MRI/derivative/ -path "*/sub-2100322004${sub_dir_name}/ses-01/anat/CIVET/mask/civet_sub-2100322004${sub_dir_name}_ses-01_T1w_type-1_derivative-brain_mask.nii.gz" -type f | head -n 1)

    if [ -n "$source_file" ] && [ -n "$dest_path" ]; then  
        cp "$source_file" "$dest_path"
        echo "Copied $source_file to $dest_path"
    else
        if [ -z "$source_file" ]; then
            echo "No nii.gz file found for $sub_dir_name"
        fi
        if [ -z "$dest_path" ]; then
            echo "No destination path found for $sub_dir_name"
        fi
    fi
done



# ADNI3                      ***************************** temp에 빼둔 거 따로 또 실행시켜야 됨 *****************************
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름
    
    # 경로에서 '*' 사용을 위해 find 명령어에 -path 옵션과 -type d를 사용
    dest_path=$(find /nasdata4/5ug/Graph_data/ADNI3/temp8/ -path "*/${sub_dir_name}/*/NIFTI" -type d | head -n 1)
    
    # source_file 경로에서도 동일하게 find 명령어를 사용
    source_file=$(find /nasdata4/CNAlab_data/BDSP/ADNI/3/ADNI/IMAGE/BRAIN/MRI/derivative/ -path "*/sub-2100332004${sub_dir_name}/ses-01/anat/CIVET/mask/civet_sub-2100332004${sub_dir_name}_ses-01_T1w_type-1_derivative-brain_mask.nii.gz" -type f | head -n 1)

    if [ -n "$source_file" ] && [ -n "$dest_path" ]; then  
        cp "$source_file" "$dest_path"
        echo "Copied $source_file to $dest_path"
    else
        if [ -z "$source_file" ]; then
            echo "No nii.gz file found for $sub_dir_name"
        fi
        if [ -z "$dest_path" ]; then
            echo "No destination path found for $sub_dir_name"
        fi
    fi
done
'''



'''
########################################################################################################################################################################################################
########################################################################################################################################################################################################
########################################################################################################################################################################################################
'''

'''
#################### output_filtered_0 file 이름 수정 ####################
find . -type f -path "*/NIFTI/output_filtered_0.nii.gz" -execdir mv {} 0_output_filtered.nii.gz \;
'''


'''
#################### files 이름 수정 ####################
for dir in */; do
    sub_dir_name=$(basename "$dir")  # 하위 디렉토리 이름

    json_file=$(find "$dir" -type f -name "DICOM_*.json")
    dti_file=$(find "$dir" -type f -name "0_output_filtered.nii.gz")
    bval_file=$(find "$dir" -type f -name "filtered.bval")
    bvec_file=$(find "$dir" -type f -name "filtered.bvec")
    t1_file=$(find "$dir" -type f -name "civet*-t1_final.nii.gz")
    
    nifti_dir=$(find "$dir" -type d -name "NIFTI")  # NIFTI 디렉토리 찾기

    if [ -n "$bval_file" ] && [ -n "$bvec_file" ] && [ -n "$dti_file" ]; then
        mv "$bval_file" "${nifti_dir}/sub-21003GO2004${sub_dir_name}_dti.bval" 
        mv "$bvec_file" "${nifti_dir}/sub-21003GO2004${sub_dir_name}_dti.bvec" 
        mv "$dti_file" "${nifti_dir}/sub-21003GO2004${sub_dir_name}_dti.nii.gz"  
        mv "$t1_file" "${nifti_dir}/sub-21003GO2004${sub_dir_name}_T1.nii.gz"
        mv "$json_file" "${nifti_dir}/sub-21003GO2004${sub_dir_name}_dti.json"

        echo "Renamed files in $dir"
    else
        echo "Missing one or more files in $dir"
    fi
done
'''

'''
#################### output* DICOM* 파일 삭제 ####################
for dir in */*/NIFTI/; do
    find "$dir" -name "DICOM_*" -exec rm -rf {} \;  # DICOM으로 시작하는 파일/디렉토리 삭제
    find "$dir" -name "output_*" -exec rm -rf {} \;  # output으로 시작하는 파일/디렉토리 삭제
    echo "Deleted DICOM_* and output_* files in $dir"
done
'''

'''
#################### 기존 마스크 삭제 ####################
for dir in */; do
    # 하위 디렉토리 이름 추출
    sub_dir_name=$(basename "$dir")  
    
    # 하위 디렉토리의 NIFTI 경로 설정
    nifti_dir=$(find "$dir" -type d -name "NIFTI")
    
    # NIFTI 디렉토리 확인
    if [ -d "$nifti_dir" ]; then
        # 해당 이름의 nii.gz 파일 경로

        nii_file="${nifti_dir}/${sub_dir_name}.nii.gz"
        
        # 파일이 존재하는지 확인하고 삭제
        if [ -e "$nifti_dir" ]; then
            rm "$nii_file"
            echo "Deleted $nii_file"
        else
            echo "No file named $nii_file found in $nii_dir"
        fi
    else
        echo "No NIFTI directory found in $dir"
    fi
done
'''
