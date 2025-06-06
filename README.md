# SAS-KOREA 데이터 분석 경진 대회
https://kdiss.or.kr/board/competition_info/article/258183
![image](https://github.com/user-attachments/assets/f3283bb6-e177-49ba-9142-931673b8fabd)

---

## Electricity Demand Forecasting Competition

> **실패로 끝난 대회?** 모델링, 분석, 리더보드 실수를 통틀어 완성한 실전형 전력 수요 예측 프로젝트

---

## Overview

* **주제**: 상권 정보 기반 전력 사용량 예측
* **참여 방식**: 실제 주최측 대회 참가 (최종 제출 1회)
* **지표**: RMSE (Root Mean Squared Error)

> 본 프로젝트는 단순 모델링을 넘어, "실전형 대회에서 어떻게 실패를 복기하고 실력을 끌어올릴 수 있는가"를 증명합니다.

---

## Modeling Summary

### 전처리 및 피처 엔지니어링

전처리 단계는 **정확한 시간 정보 활용**, **범주형 변수 정제**, **모델이 이해할 수 있는 수치 기반 파생**을 목표로 세심하게 설계했습니다.

#### 주요 처리 및 이유

* `DATA_YM` → `YEAR`, `MONTH`, `SEASON`으로 분해:

  * 월별/계절별 소비 패턴 반영 (전기 사용량은 계절성과 밀접함)
* `AREA_ID`, `DIST_CD`, `AREA_DIST_ID` 등은 범주형 변수로 유지:

  * 지역별 고정 효과를 잡기 위해 One-hot 인코딩 대신 Category 처리 유지
* `FAC_TOTAL`, `FAC_DENSITY` 등 시설 기반 파생:

  * 상권의 규모와 시설 밀집도는 소비 패턴에 직접적 영향
* `RETAIL_RATIO`, `GAS_RATIO`, `MEDI_RATIO` 등 비율 파생:

  * 전체 규모 대비 특정 기능이 강한 상권을 반영함
* `MEAN_ELEC_BY_AREA`, `AREA_ID_FREQ` 등 통계 기반 피처:

  * 지역 평균 소비 패턴/데이터 희소도 정보를 활용해 일반화 보조

### 타겟 처리

* `TOTAL_ELEC`의 분포가 매우 비대칭(positive skewed)이므로 `log1p` 변환하여 모델 안정성 확보

---

### 모델 구조

* `LGBMRegressor` + `XGBRegressor` 앙상블 (평균 기반)
* `KFold(n_splits=5)` 기반 검증으로 과적합 방지
* 범주형 변수는 LGBM에선 그대로, XGB에는 `.cat.codes()` 방식 적용

### 교훈: expm1 + clip 미적용

* `np.expm1()` 후 일부 예측값이 4000 초과 → RMSE 폭등
* Validation RMSE: **136** → 실제 제출 RMSE: **480+**

---

## 주요 성과 요약

| 항목              | 결과         |
| --------------- | ---------- |
| Validation RMSE | **136.60** |
| 과적합률 평균         | 67.47%     |
| Train RMSE 평균   | 91.57      |
| 예측 최대값 (미클리핑)   | 4,316.06   |

*분석적 사고 + 코드 설계 + 결과 복기까지 모두 포함된 프로젝트입니다.*

---

## 기술 스택

![Python](https://img.shields.io/badge/Python-3.10-blue)
![LightGBM](https://img.shields.io/badge/LightGBM-ensemble-green)
![XGBoost](https://img.shields.io/badge/XGBoost-regression-orange)
![Scikit-learn](https://img.shields.io/badge/sklearn-cv-lightgrey)

* Python, Pandas, NumPy
* LightGBM, XGBoost
* Scikit-learn (CV, metrics)
* Seaborn, Matplotlib (시각화)

---

## 폴더 구조

```
electricity-demand-forecasting
├── data/                  # 원본 데이터 비공개 처리
├── notebooks/             # EDA 및 모델링 노트북
│   ├── EDA.ipynb
│   └── modeling_lgb_xgb.ipynb
├── report/                # 최종 보고서
│   └── final_report.md (또는 PDF)
├── submission/            # 제출 파일
│   └── WITH AI_최종.csv
└── README.md
```

---

## 학습 포인트

* `log1p`/`expm1`과 분포 통제의 중요성
* 실전 앙상블 구성 및 과적합 관리 전략
* 실수에서 배우고, 그 과정을 복기할 수 있는 구조화된 리포팅 습관

---

## 최종 회고

> “Validation RMSE 136이었지만, clip 한 줄 없었다는 이유로 13등을 놓쳤습니다. 그러나 이 경험이 저를 실전형 모델러로 성장시켰습니다.”

---

## 보고서

[최종 보고서 보기](./report/final_report.md)

---

> © 2025. 오지송. All rights reserved.

