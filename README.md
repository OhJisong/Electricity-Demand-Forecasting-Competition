# SAS-KOREA 데이터 분석 경진 대회
https://kdiss.or.kr/board/competition_info/article/258183
![image](https://github.com/user-attachments/assets/f3283bb6-e177-49ba-9142-931673b8fabd)

---

## Electricity Demand Forecasting Competition

> **모델링, 분석, 리더보드 실수를 통틀어 완성한 실전형 전력 수요 예측 프로젝트**

---

## Overview

* **주제**: 상권 정보 기반 전력 사용량 예측
* **참여 방식**: 실제 주최측 대회 참가 (최종 제출 1회)
* **지표**: RMSE (Root Mean Squared Error)

> 본 프로젝트는 단순 모델링을 넘어, "실전형 대회에서 어떻게 실패를 복기하고 실력을 끌어올릴 수 있는가"를 증명합니다.

---

## Modeling Summary

### 전처리 및 파생

* `DATA_YM` → `YEAR`, `MONTH`, `SEASON` 파생
* 범주형 변수 다중 인코딩
* 통계 기반 파생: `MEAN_ELEC_BY_AREA`, `AREA_ID_FREQ` 등
* 타겟 로그변환: `LOG_TOTAL_ELEC = np.log1p(TOTAL_ELEC)`

### 모델 구조

* `LGBMRegressor` + `XGBRegressor` 앙상블 (평균 기반)
* `KFold(n_splits=5)` 기반 검증
* 카테고리형은 LGBM에 그대로, XGB에는 `.cat.codes()` 적용

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
* 실패한 제출이라도 충분히 분석하고 다음에 반영할 수 있다면 그것은 실패가 아니다.

---

## 최종 회고

> “Validation RMSE 136이었지만, clip 한 줄 없었다는 이유로 13등을 놓쳤습니다. 그러나 이 경험이 저를 실전형 모델러로 성장시켰습니다.”

---

## 보고서

[최종 보고서 보기](./report/final_report.md)

---

> © 2025. 오지송. All rights reserved.
