---
sort: 3
published: true
---
# Clearing

```note
- 업로드 되면서 Scan 된 결과에는 Dual License인 파일이나, False Alarm인 파일들이 있을 수 있습니다.
- 이에, Clearing을 통해 Scanning 결과를 정제하여 보다 더 정확한 정보를 얻을 수 있습니다.
```

## Clearing의 필요성

### Dual License

FOSSology는 String Search를 기반으로 하기에, 하나의 파일에 여러 License의 Keyword가 발견되면, 이를 모두 표기해줍니다. 이렇게 Dual License된 파일의 경우 사용자는 해당 파일에 검출 결과 중 사용할 License로 적절히 Identify 해야 합니다.

- Dual License : 한 File을 여러 License 중 하나로 사용할 수 있는 경우. (e.g. BSD-3-Clause OR GPL-2.0인 libcap은 사용자가 BSD-3-Clause와 GPL-2.0 두 License 중 하나를 선택하여 사용할 수 있음)

### False Alarm

FOSSology는 String Search를 기반으로 하기에, 파일에 포함되어있는 Keyword에 따라 잘못된 License가 검출될 수 있습니다. 이러한 경우 사용자는 잘못 검출된 License를 제거하여야 합니다.

- 예시의 파일(Example_Project.zip/Source/MediaInfo/MediaInfo_Config_Automatic.cpp)의 경우 BSD, GPL 두 가지의 License가 검출되었습니다.
- 하지만, 실제 주석에서는 해당 파일이 BSD-style임이 명시되어있고, Code 내에 String으로 'GNU GPL'이 포함되어 FOSSology에서는 False Alarm이 발생하게 됩니다.

![Branching](img_3_clearing/01_3_False_Alarm_1.png)

![Branching](img_3_clearing/01_4_False_Alarm_2.png)

## Clearing 방법

### 파일  단위 Clearing

```note
사용자가 각 파일단위로 직접 Identify 할 수 있습니다.
```

![Branching](img_3_clearing/02_1_File_Clearing.png)

1. 검색된 License 추가 / 삭제
  - User Decision에서 License를 검색하여 추가
  - Action의 추가 버튼 사용하여 License 삭제
2. Clearing Decision Type
  - Identified : License 확인이 완료되었음
  - Irrelevant : Open Source가 사용되지 않은 파일임
3. Submit
  - Clearing 결과를 저장검색된 License 추가 / 삭제

### Bulk Clearing

```note
동일한 Text를 포함하는 모든 File들에 대해 License를 추가 / 삭제 할 수 있습니다.
```

![Branching](img_3_clearing/02_2_Bulk_Clearing.png)

1. License 선택
  - 동작을 취할 License 선택
2. Add/Remove License
  - 선택된 License에 대해 취할 동작 선택
3. Reference Text
  - 검색할 Text 입력
4. Schedule Bulk Scan
  - 1,2,3 에서 지정한 동작을 Job 스케쥴에 추가

## Clearing 결과 확인

### License Browser

```note
License Browse에서는 각 파일의 Clearing 상태를 확인할 수 있습니다.
- Clearing Status : Clearing 완료 여부를 표현합니다. 녹색은 파일 혹은 디렉토리 전체의 Clearing이 완료되었음을, 빨간색은 아직 완료되지 않았음을 뜻합니다.
- Files Cleared : 총 파일 수와 Clearing이 완료된 파일 수를 표현합니다. (완료된 파일 수 / 총 파일 수)
```

![Branching](img_3_clearing/03_1_Check_License_Browser.png)

### License List

```note
다운로드 받은 License List (CSV 파일)에서 각 파일별 Clearing 결과를 확인할 수 있습니다. ([4. OSS List 작성](4_oss-report.html) 참고)
- file path : 각 파일의 Path
- scan results : FOSSology가 검출한 License
- concluded results : Clearing 결과
```

![Branching](img_3_clearing/03_2_Check_License_List.png)
