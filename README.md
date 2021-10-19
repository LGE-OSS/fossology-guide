# FOSSology

## FOSSology란?

FOSSology는 String Search를 기반으로 한 Open Source 분석 Web Server 어플리케이션입니다.

## 특징
- GPL-2.0으로 배포되는 Open Source로, 무료로 사용할 수 있고, Source Code 수정이 가능합니다.
- String Search를 기반으로 합니다.
  - 각 File별 License 정보를 확인하는 Tool로써 License 확인이 명확합니다.
  - License 문구 및 키워드를 기준으로 String Search 하기 때문에, 개발자가 각 File 내 관련 License 문구 및 키워드를 삭제할 경우 검출이 불가능합니다.
  - 파일 내에 특정 키워드가 포함되면 False Alarm이 발생합니다. 예를 들어 Source File 내 함수 이름이 GPL() 이거나, JPG등 Resource Data 내 우연히 GPL 이라는 String이 존재할 경우 해당 File을 GPL로 판단합니다.

## 관련 링크
- 홈페이지 : [https://www.fossology.org](https://www.fossology.org)
- 배포처 : [https://github.com/fossology/fossology](https://github.com/fossology/fossology)
- 공식 가이드 : [https://www.fossology.org/get-started](https://www.fossology.org/get-started)
- 개발자 가이드 : [https://github.com/fossology/fossology/wiki](https://github.com/fossology/fossology/wiki)

# License
Copyright (c) 2020 LG Electronics.
- Documents in this site are licensed under the [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode)
