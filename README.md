# 🧾 Rossmann Store Sales Forecasting  
3,000여 개 Rossmann 드럭스토어의 **일일 매출을 6주 앞까지 예측하는 프로젝트**입니다.  
시계열 모델(SARIMA/SARIMAX), XGBoost 회귀 모델, Prophet 모델을 비교하여 예측 성능을 평가합니다.

---

# 🎯 프로젝트 목적
Rossmann 매장의 **일일 매출(Sales)** 데이터를 기반으로:

- 시계열 패턴(추세·계절성)
- 매장 정보(StoreType, Assortment, Competition)
- 프로모션(Promo, Promo2)
- 휴일 효과(StateHoliday, SchoolHoliday)

등을 종합해 **정확한 미래 매출을 예측하는 모델을 구축**합니다.

---

# 🧩 문제 정의
- `Sales`는 강한 **계절성**, **요일 패턴**, **프로모션 영향**, **경쟁 매장 거리 영향**이 존재  
- 일부 매장은 리모델링으로 **영업일(Open=0)**  
- 목표: **테스트 데이터의 Sales를 예측하여 제출 파일 생성**

---

# 📊 데이터셋 설명

### 사용된 데이터 파일
- **train.csv** — 매출(Sales) 포함 학습 데이터  
- **test.csv** — 매출 제외, 예측 대상  
- **store.csv** — 매장 추가 정보  
- **sample_submission.csv** — 제출 형식 참고  

### 주요 컬럼 요약
- **Store**: 매장 ID  
- **Sales**: 일일 매출(예측 대상)  
- **Customers**: 방문 고객 수  
- **Open**: 매장 영업 여부  
- **StateHoliday, SchoolHoliday**: 공휴일/학교 휴일 영향  
- **StoreType, Assortment**: 매장 유형 / 상품 구색  
- **CompetitionDistance**: 가장 가까운 경쟁사 거리  
- **Promo, Promo2**: 프로모션 정보  

---

# 🔍 EDA 요약 (Exploratory Data Analysis)

- 매출은 **요일별 패턴**이 뚜렷하며 일요일 매출은 매우 낮음  
- **Promo=1**일 때 매출이 전반적으로 상승  
- **CompetitionDistance**가 가까울수록 매출 감소 경향  
- ** 계절성(Seasonality)** + **추세(Trend)** 존재 확인  
- ACF/PACF 분석 결과 ARIMA/SARIMA 적용 필요  

---

# 🧠 모델링 전략
| 모델 | 목적 | 장점 |
|------|------|-------|
| **SARIMA/SARIMAX** | 순수 시계열 패턴 예측 | 계절성·추세 반영에 강함 |
| **XGBoost Regressor** | Feature 기반 회귀 | 매장별 Feature 활용 가능 |
| **Prophet** | 휴일·계절성 자동 감지 | 복잡한 전처리 없이 시계열 예측 |

---

# 📈 결과 비교 (예시)
| Model | RMSE | Notes |
|-------|-------|--------|
| SARIMA | - | 계절성 반영 |
| XGBoost | - | Feature 기반 예측에서 우수 |
| Prophet | - | 휴일 효과 반영 |

---

# 📁 프로젝트 구조
<details>
<summary><strong>펼치기 / 접기</strong></summary>



# 📁 프로젝트 구조

<details>
<summary><strong>📦 rossmann-sales-prediction (click to expand)</strong></summary>

<br>

📦 rossmann-sales-prediction  
├── 📂 data  
│   ├── 📄 train.csv  
│   ├── 📄 test.csv  
│   ├── 📄 store.csv  
│   └── 📄 sample_submission.csv  
│  
├── 📂 notebooks  
│   ├── 📘 EDA.ipynb  
│   ├── 📘 TimeSeries_ARIMA.ipynb  
│   ├── 📘 XGBoost_Modeling.ipynb  
│   └── 📘 Prophet_Modeling.ipynb  
│  
├── 📂 src  
│   ├── 🔧 utils.py  
│   ├── 🔧 preprocess.py  
│   ├── 🔧 train_sarima.py  
│   ├── 🔧 train_xgboost.py  
│   └── 🔧 train_prophet.py  
│  
├── 📂 outputs  
│   ├── 📄 predicted_arima.csv  
│   ├── 📄 predicted_xgb.csv  
│   └── 🖼️ model_compare.png  
│  
├── 📄 requirements.txt  
└── 📄 README.md  

</details>

---

# 🧰 기술 스택 (Technology Stack)

<details>
<summary><strong>📌 Models</strong></summary>

<br>

📦 **Models**  
├── 🔹 **SARIMA / SARIMAX** — 통계 기반 시계열 모델  
│   └── statsmodels.api (package)  
│       └── SARIMAX (class) → 계절성·추세 반영 ARIMA 확장 모델  
│  
├── 🔹 **XGBoost Regressor** — 트리 기반 Gradient Boosting 회귀  
│   └── xgboost (package)  
│       ├── sklearn (module)  
│       │   └── XGBRegressor (class) → 회귀 모델  
│       └── core (module)  
│           └── DMatrix (class) → XGBoost 학습 전용 데이터 구조  
│  
└── 🔹 **Prophet** — Meta 개발 자동 계절성 기반 시계열 모델  
    └── prophet (package)  
        └── Prophet (class) → 추세·계절성·휴일 효과 학습  

</details>

---

<details>
<summary><strong>📌 Machine Learning Algorithms</strong></summary>

<br>

📦 **Machine Learning Algorithms**  
├── 🔹 **scikit-learn** — 회귀/분류, 데이터 분할, 평가  
│   ├── model_selection  
│   │   └── train_test_split() → 학습/검증 데이터 분할  
│   ├── metrics  
│   │   ├── mean_squared_error()  
│   │   └── r2_score() → RMSE / R² 평가  
│   └── preprocessing  
│       └── LabelEncoder → 범주형 인코딩  
│  
└── 🔹 **xgboost** — Gradient Boosting Tree 학습  
    ├── xgboost.sklearn  
    │   └── XGBRegressor (class)  
    └── xgboost.core  
        └── DMatrix (class) → 고속 학습 데이터 구조  

</details>

---

<details>
<summary><strong>📌 Time Series Analysis</strong></summary>

<br>

📦 **Time Series Analysis**  
├── statsmodels.tsa.stattools  
│   ├── adfuller() → 정상성 검정  
│   ├── acf() → 자기상관  
│   └── pacf() → 편자기상관  
│  
├── statsmodels.tsa.seasonal  
│   └── seasonal_decompose() → 시계열 분해(Trend/Seasonality)  
│  
├── statsmodels.tsa.arima_model  
│   └── ARIMA (class) → 전통 ARIMA 모델  
│  
└── statsmodels.api  
    └── SARIMAX (class) → SARIMA/SARIMAX 모델  

</details>

---

<details>
<summary><strong>📌 Data Processing</strong></summary>

<br>

📦 **Data Processing**  
├── pandas  
│   ├── DataFrame (class)  
│   ├── read_csv()  
│   └── groupby(), merge(), pivot_table()  
│  
└── numpy  
    ├── array (class)  
    ├── arange(), linspace()  
    └── 벡터 연산(vectorized operations)  

</details>

---

<details>
<summary><strong>📌 Utility & System Tools</strong></summary>

<br>

📦 **Utility Tools**  
├── os → 파일/디렉토리 접근  
├── time → 실행 시간 측정  
├── warnings → 경고 숨기기  
└── datetime → 날짜/시간 처리  


</details>

## 📑 Data Fields (컬럼 상세 설명)

<details>
<summary><strong>📌 Click to expand (컬럼 상세 설명 펼치기)</strong></summary>

<br>

📦 <strong>기본 식별자</strong>  
├── <code>Id</code>  
│     └─ 테스트 세트 내 (Store, Date) 조합을 나타내는 고유 ID  
└── <code>Store</code>  
      └─ 각 매장을 구분하는 고유 ID  

---

📦 <strong>매출 및 고객 정보</strong>  
├── <code>Sales</code>  
│     └─ 특정 날짜의 매출 (예측 타깃 값)  
└── <code>Customers</code>  
      └─ 방문 고객 수  

---

📦 <strong>영업 여부</strong>  
└── <code>Open</code>  
      └─ 매장 영업 상태  
         • 0 = 휴점  
         • 1 = 영업  

---

📦 <strong>공휴일 정보</strong>  
├── <code>StateHoliday</code>  
│     ├─ a = 공휴일(public holiday)  
│     ├─ b = 부활절(Easter)  
│     ├─ c = 크리스마스  
│     └─ 0 = 해당 없음  
│        → 대부분 매장은 공휴일에 휴점  
│        → 모든 학교는 공휴일 + 주말에 휴교  
└── <code>SchoolHoliday</code>  
      └─ 공립학교 휴교 여부  

---

📦 <strong>매장 유형 및 구색</strong>  
├── <code>StoreType</code>  
│     └─ 매장 유형(a, b, c, d)  
└── <code>Assortment</code>  
      ├─ a = Basic (기본)  
      ├─ b = Extra (추가)  
      └─ c = Extended (확장)  

---

📦 <strong>경쟁 매장 정보</strong>  
├── <code>CompetitionDistance</code>  
│     └─ 가장 가까운 경쟁 매장까지의 거리(미터)  
└── <code>CompetitionOpenSince[Month/Year]</code>  
      └─ 경쟁 매장이 개점한 대략적인 연·월  

---

📦 <strong>프로모션 정보</strong>  
├── <code>Promo</code>  
│     └─ 해당 날짜에 1차 프로모션 진행 여부 (0/1)  
│  
└── <code>Promo2</code>  
      ├─ 지속적/순환형 프로모션 프로그램  
      ├─ 0 = 미참여  
      ├─ 1 = 참여  
      │  
      ├── <code>Promo2Since[Year/Week]</code>  
      │     └─ Promo2 참여 시작 연도 및 주(week)  
      │  
      └── <code>PromoInterval</code>  
            └─ Promo2가 매년 시작되는 월 목록  
               예: <code>Feb, May, Aug, Nov</code>  
               → 매년 2·5·8·11월에 프로모션 라운드가 새로 시작됨  

</details>


