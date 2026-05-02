# 데이콘 제 2회 학습자 수료 예측 AI 경진대회

## 1. 프로젝트 설명
- 목표: BDAI 학회의 학회원 데이터를 활용해 학습자의 **수료 여부(0/1)** 를 예측한다.
- 평가 지표: Binary F1 Score
- 대회 기간: 2026.01.12 - 2026.02.23
- 결과: F1 score: 0.42

## 2. 프로젝트 파이프라인
1) Data Load
2) EDA (분포 및 결측 확인)
3) Preprocess (결측 처리, 파생변수 생성, Bool 변환, 다중값 처리)
4) Feature Selection (변수 중요도, p-value 검정)
5) Model (5-fold + Optuna: RF/XGB/CAT + 블렌딩)
6) Predict

## 3. 디렉토리 구조
```
.
├─ assets/                # EDA 시각화 결과
├─ data/
│  ├─ raw/                # 원본 데이터
│  └─ processed/          # 전처리 산출물
├─ outputs/               # 제출파일
│  └─ submission.csv
├─ src/
│  ├─ preprocess.ipynb    # 데이터 전처리
│  └─ v18.ipynb           # 모델 학습/검증 및 제출 파일 생성(최종)
├─ requirements.txt       # 최소 실행 패키지 목록
└─ README.md
```

