# 학습자 수료 예측 AI 경진대회

- 데이콘 x BDA 제 2회 학습자 수료 예측 AI 경진대회
- 주최: 데이콘 x BDA
- 대회 기간: 2026.01.12 – 2026.02.23  
- 평가 지표: Binary F1 Score  
- 최종 점수: F1 = 0.42

---

## 1. 프로젝트 개요

BDAI 학회원 데이터를 기반으로 학습자의 수료 여부(0/1)를 예측하는 이진 분류 문제.  
- 학습 데이터: 9기 학회원 데이터 (completed 레이블 포함)  
- 예측 대상: 10기 학회원 데이터 (completed 예측)

---

## 2. 사용 데이터

| 파일 | 설명 |
|------|------|
| train.csv | 9기 학회원 정보 + completed 레이블 |
| test.csv | 10기 학회원 정보 |

---

## 3. 프로젝트 파이프라인

```
Data Load
    │
    ▼
EDA (분포 및 결측 확인)
    │
    ▼
Preprocess
  - 파생변수 생성 (count_class, previous_class_count)
  - Bool 변환 (re_registration, contest_participation, major_type)
  - 결측치 처리 (숫자형: 중앙값, 범주형: 'Unknown')
  - 다중값 처리 및 LabelEncoding
    │
    ▼
Feature Selection (v14.ipynb)
  - 숫자형: Mann-Whitney U test
  - 범주형: Chi-squared test
  - p < 0.05 유의 피처만 선택 (경계선 p < 0.1 포함)
    │
    ▼
Model (StratifiedKFold 5-fold + Optuna TPESampler)
  - RandomForest (80 trials)
  - XGBoost (80 trials)
  - CatBoost (60 trials)
  - 균등 블렌딩 (RF + XGB + CAT) / 3
    │
    ▼
Threshold Tuning
  - OOF 예측 확률 기반 F1 최대화 threshold 탐색
  - 비율 기반 예측: train 양성 비율 기준 상위 N개를 1로 분류
    │
    ▼
Predict & Submit
```

---

## 4. 피처 선택 결과 (`v14.ipynb`)

통계 검정(p < 0.05) 기준 유의 피처 9개:

| 피처 | 유형 | 검정 방법 |
|------|------|-----------|
| previous_class_count | 숫자형 | Mann-Whitney |
| re_registration | 숫자형 | Mann-Whitney |
| major_data | 숫자형 | Mann-Whitney |
| certificate_acquisition | 범주형 | Chi-squared |
| whyBDA | 범주형 | Chi-squared |
| inflow_route | 범주형 | Chi-squared |
| major1_2 | 범주형 | Chi-squared |
| count_class | 숫자형 | p < 0.1 (경계선 포함) |
| hope_for_group | 범주형 | p < 0.1 (경계선 포함) |

---

## 5. 모델 학습 설정

- 교차 검증: StratifiedKFold (n_splits=5)
- 하이퍼파라미터 탐색: Optuna TPESampler
- 블렌딩: RF / XGB / CatBoost OOF 확률 균등 평균
- Threshold: OOF F1 최대화 기준 탐색 

---

## 6. 디렉토리 구조

```
.
├─ assets/                # EDA 시각화 결과
├─ data/
│  ├─ raw/                # 원본 데이터
│  └─ processed/          # 전처리 산출물 (train_v8.csv, test_v8.csv)
├─ outputs/
│  └─ submission.csv      # 최종 제출 파일
├─ src/
│  ├─ Preprocess.ipynb    # 데이터 전처리 및 파생변수 생성
│  └─ modeling.ipynb           # 피처 선택 + 모델 학습 + 제출 파일 생성 (최종)
├─ requirements.txt
└─ README.md
```
