- 폴더 단체 생성

```
mkdir B11013 B11019 B11021 …
```

- 모든 폴더 돌며 하위 폴더 생성

```
for folder in */; do mkdir -p "$folder"/animal "$folder"/WMH "$folder"/hippocampus "$folder"/hdbet; done
for dir in */combat_result/; do mkdir -p "$dir"/{harmonized,neuroCombat,unharmo_thickness}; done
```

- 모든 하위 폴더 내부의 모든 폴더 삭제

```
find . -mindepth 2 -maxdepth 2 -type d -exec rm -rf {} \;
```

- 타겟 폴더로부터 현재 경로의 하위 폴더의 이름이 포함된 파일들 정리

```
for folder in */; do
    folder_name=$(basename "$folder")
    mv /Users/jiugeun/Documents/fbf/haca3/F/*"$folder_name"* "$folder"
done
```
