# 개인 맞춤형 운동 루틴 추천 서비스

#### 서비스 개요
- 국민체력측정 처방 정보를 분석하여 개인별 신체정보와 식단에 따른 **맞춤 운동 루틴 추천**
- 처방 운동 동영상 가이드 연결을 통해 운동에 익숙하지 않은 사용자도 **올바른 운동 자세 학습 가능**
- **운동 기록과 성취도를 평가**하고 달력을 통해 운동 기록을 한눈에 확인

## 목차
1. [Directory](#1-directory)
2. [분석 프로세스](#2-분석-프로세스)
3. [데이터 수집](#3-데이터-수집)
4. [데이터 전처리](#4-데이터-전처리)
5. [추천 알고리즘](#5-추천-알고리즘)

## 1. directory
```
├─Data :   
│ ├─Processing : 전처리 데이터  
│ ├─Raw    
│ └─Sample : 테스트용 샘플 데이터    
├─Code :   
│ ├─01_Preprocessing.ipynb : 전처리 파일  
└─┴─02_Modeling.ipynb : 운동 추천 및 샘플 프로토타입    
```

## 2. 분석 프로세스
① 항목별 체력측정 데이터와 운동처방 데이터를 통해 통합 데이터셋 생성

② 통합 데이터셋을 통해 사용자 유형별 처방 횟수 데이터셋 생성

③ 각 운동별 희소행렬 도출

④ 연관 분석을 통해 사용자별 추천 운동 리스트 도출

⑤ 사용자에게 맞는 운동 세트(준비운동, 본운동, 마무리운동) 추천  

### 분석 흐름도
<img src="https://github.com/soohyunme/exercise/blob/main/img/1.png?raw=true"/>  

## 3. 데이터 수집
- [체력측정 항목별 측정 데이터](https://www.culture.go.kr/bigdata/user/data_market/detail.do?id=ace0aea7-5eee-48b9-b616-637365d665c1#!) - 문화 빅데이터 플랫폼 [2017.01 ~ 2021.12]
- [체력 측정별 운동처방 데이터](https://www.culture.go.kr/bigdata/user/data_market/detail.do?id=2b1c565d-5f37-4152-966d-5f8094f8cf33) - 문화 빅데이터 플랫폼 [2017.01 ~ 2021.12]

## 4. 데이터 전처리
① 불필요한 열 Drop 처리  
- 서비스에 필요한 열 이외의 측정 센터명, 입력 구분, 측정일 등 불필요한 열 제거

② 통합 데이터셋 생성  
- 항목별 측정 데이터와 운동처방 데이터 병합

③ 운동처방 데이터 정제  
- 운동처방 열을 준비운동, 본운동, 마무리운동으로 분리
- 추천 운동이 존재하지 않는 행 제거
- 운동명 통일

④ 결측치 및 이상치 제거
- 신체 특성에 중요한 상장구분, 신장, 몸무게의 결측치 항목 제거
- 신장, 몸무게, BMI 수치를 통해 이상치 항목 제거

⑤ 사용자 유형 데이터셋 생성
- 연령대, BMI 단계, 운동 강도를 통해 유형을 분류하여 데이터셋 생성

⑥ 통합 데이터셋 생성
- 사용자 데이터와 유형 ID 추가, 데이터셋 생성

## 5. 추천 알고리즘
① 사용자의 신체 정보를 이용해 사용자 유형 데이터 생성
<img src="https://github.com/soohyunme/exercise/blob/main/img/2.png?raw=true"/>  

② 유형 정보를 통해 최다 처방세트(*해당 유형에 가장 많이 처방된 운동 조합), 각 운동별 희소행렬 도출
<img src="https://github.com/soohyunme/exercise/blob/main/img/3.png?raw=true"/>  

③ 희소행렬을 통해 지지도 및 신뢰도 도출, 최다 처방 운동의 지지도 상위 3개 운동을 연관 운동 리스트에 추가
<img src="https://github.com/soohyunme/exercise/blob/main/img/4.png?raw=true"/>  

④ 최다 처방세트에서 단계별로 추천운동 1개를 선정, 선정되지 않은 운동과 연관 운동 리스트의 운동들로 추가 추천 운동 선정
<img src="https://github.com/soohyunme/exercise/blob/main/img/5.png?raw=true"/>  

⑤ 추천 운동 및 추가 추천 운동 정보 제공
<img src="https://github.com/soohyunme/exercise/blob/main/img/6.png?raw=true"/>  


