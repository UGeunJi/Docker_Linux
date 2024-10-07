## Animal -> csv file

### ANIMAL segmentation
```
Right parietal gray matter
Left lateral ventricle
Right occipital gray matter
Left parietal gray matter
Left occipital gray matter
Right lateral ventricle
Right globus pallidus
Left globus pallidus
Left putamen
Right putamen
Right frontal white matter
Brainstem
Right subthalamic nucleus
Left fornix
Left frontal white matter
Left subthalamic nucleus
Left caudate
Right occipital gray matter
Right caudate
Left parietal white matter
Right temporal white matter
Left cerebellum
Left occipital white matter
Right cerebellum
Left temporal white matter
Left thalamus
Right parietal white matter
Right thalamus
Left frontal gray matter
Right frontal gray matter
Left temporal gray matter
Right temporal gray matter
3rd ventricle
4th ventricle
Right fornix
Extracerebral CSF
```


### 명령어
```
for dat_file in *.dat; do
    # dat 파일 이름에서 'N' 뒤에 오는 7자리 숫자 추출
    output_prefix=$(echo "$dat_file" | grep -o 'N[0-9]\{6\}')
    
    # 출력 확인
    echo ${output_prefix}
    
    # 1. dat 파일에서 숫자와 값을 추출하여 임시 파일에 저장 (1번째부터 255번째까지)
    awk 'NR >= 1 && NR <= 255 {print $1 "," $2}' "$dat_file" > temp_numbers_values.csv

    # 2. dat 파일의 1~255번째 숫자들을 첫 번째 행으로 설정
    awk 'NR >= 1 && NR <= 255 {printf "%s,", $1}' "$dat_file" | sed 's/,$//' > header_numbers.csv

    # 3. 두 번째 행 생성 (숫자에 맞는 해부학적 부위들)
    echo "Right parietal gray matter,Left lateral ventricle,Right occipital gray matter,Left parietal gray matter,Left occipital gray matter,Right lateral ventricle,Right globus pallidus,Left globus pallidus,Left putamen,Right putamen,Right frontal white matter,Brainstem,Right subthalamic nucleus,Left fornix,Left frontal white matter,Left subthalamic nucleus,Left caudate,Right occipital gray matter,Right caudate,Left parietal white matter,Right temporal white matter,Left cerebellum,Left occipital white matter,Right cerebellum,Left temporal white matter,Left thalamus,Right parietal white matter,Right thalamus,Left frontal gray matter,Right frontal gray matter,Left temporal gray matter,Right temporal gray matter,3rd ventricle,4th ventricle,Right fornix,Extracerebral CSF" > header_anatomical_regions.csv

    # 4. 세 번째 행은 dat 파일에서의 값 (1~255번 값들)
    awk 'NR >= 1 && NR <= 255 {printf "%s,", $2}' "$dat_file" | sed 's/,$//' > values.csv

    # 5. 최종 CSV 파일로 합침 (파일 이름에 추출한 7자리 숫자 포함)
    paste -d '\n' header_numbers.csv header_anatomical_regions.csv values.csv > "${output_prefix}_output.csv"

    # 6. 임시 파일 삭제
    rm temp_numbers_values.csv header_numbers.csv header_anatomical_regions.csv values.csv
done
```
