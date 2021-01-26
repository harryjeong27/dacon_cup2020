# Dacon Cup 2020
> Analyze and predict user pattern by using user data

# Table of Contents
1. [Problem Definition & Domain Research](#ProblemDefinition&DomainResearch)  
    1.1 [Problem Definiton](#problemdefinition)  
    1.2 [Data](#data)  
2. [Acquire training and testig data : Data Loading](#dataloading)  
    2.1 [Package Loading & Basic Setting](#package)  
    2.2 [Data Loading](#loading)  
3. [Data Analyze (EDA) & Preprocessing (Wrangle, Cleanse)](#EDA&Wrangling)  
    3.1 [Analyze by describing data (Quick-view)¶](#quick)  
    3.2 [Analyze by visualizing data](#visual)  
4. [Modeling, Predict and Solve the problem](#modeling)  
    4.1 [Listing possible model](#modellisting)  
    4.2 [Modeling](#predicting)  

# 1. Problem Definition & Domain Research<a name="ProblemDefinition&DomainResearch"></a>
## 1.1 Problem Definiton<a name="problemdefinition"></a>
### 1.1.1 Topic
- 과거의 데이콘 데이터를 활용한 미래의 사용자 행동 패턴을 예측

### 1.1.2 Background
- 전체 회원 약 2만명, 약 3만 회의 대회 참여, 47개의 공식 대회 개최, 총 상금 3억 7천만원이라는 국내 최대 규모의 인공지능 컴피티션 플랫폼데이콘입니다.
- 2020년 한 해를 마무리하며 데이콘의 사용자 행동 데이터를 바탕으로 하는 대회를 주최합니다.

### 1.1.3 Purpose 
- 지난 2년 동안의 데이콘에 관한 여러가지 데이터가 주어집니다. 이 데이터를 통해서 사용자 움직임을 예측하고자 합니다.
- 2018.09.09 ~ 2020.12.08 기간의 데이터로 이후 30일 간의 데이터 예측

### 1.1.4 Host
- Dacon

## 1.2 Data<a name="data"></a>
### 1.2.1 Dictionary
- 2018.09.09 ~ 2020.12.08 기간동안 기록된 한시간 간격의 사용자 행동 데이터
    - 사용자 수 : 
    - 세션 수 : 모든 사용자가 시작한 개별 세션 수
      - 사용자가 사이트를 방문한 후 30분 이상 어떤 작업도 수행하지 않은 경우 이후에 발생한 모든 작업은 새로운 세션으로 간주
      - 사용자가 사이트를 떠났다가 30분 내에 다시 방문하면 해당 방문은 원래 세션의 일부로 기록
      - 언제든지 최초 세션의 방문자는 새로운 세션 및 새로운 사용자로 간주
      - 이 기간에 동일한 사용자가 추가로 발생시키는 세션은 추가 세션으로 집계되지만, 추가 사용자로는 집계되지 않음
    - 신규 방문자 수
    - 페이지 뷰 수

# 2. Acquire training and testig data : Data Loading¶<a name="dataloading"></a>
## 2.1 Package Loading & Basic Setting<a name="package"></a>
    import os
    import warnings
    warnings.filterwarnings('ignore')
## 2.2 Data Loading<a name="loading"></a>
- 여러 데이터셋이 있으나, 핵심인 모델에 넣을 train, 2차_train 데이터셋만 로딩
- 시각화를 통한 데이터 파악을 위해 두 개의 데이터 결합

# 3. Data Analyze (EDA) & Preprocessing (Wrangle, Cleanse)<a name="EDA&Wrangling"></a>
## 3.1 Analyze by describing data (Quick-view)¶<a name="quick"></a>
### 3.1.1 Check columns (name)
### 3.1.2 Check feature type
1) Categorical
- Categorical
- Ordinal  

2) Numerical
- Continous : 사용자, 세션, 신규방문자, 페이지뷰
- Discrete
### 3.1.3 Check blank, null or empty values & data types
- integer or floats or strings(objects)  
    df.info()
- no NULL
- **DateTime은 datetime으로 변경해주기**
### 3.1.4 Check distribution of numerical feature values
    df.describe()
## 3.2 Analyze by visualizing data<a name="visual"></a>
>Confirming some of our assumptions using visualizations for analyzing the data.

### Summary
- 사용자, 세션, 신규방문자, 페이지뷰는 어느정도 차이가 있긴하지만 비슷한 추세를 보임을 확인
- 2019년 10, 11월 기점으로 빠르게 그래프가 우상향
- 2019년 12월에 주춤, 2020년 8월에 대폭 상승하는 모습을 보여줌
    - 연말 여파로 잠시 줄어들었다가 1월에 새해 마음가짐으로 인해 다시 상승하는 것으로 예상됨
    - 학생들이 많을 것으로 예상되는 플랫폼이므로 방학인 8월에 상승하는 모습을 보이지 않을까 예상됨
- 모든 컬럼이 아주 강한 상관관계를 가지고 있음
### 시계열 선 그래프
- 사용자, 세션, 신규방문자, 페이지뷰는 어느정도 차이가 있긴하지만 비슷한 추세를 보임을 확인
- 2019년 10, 11월 기점으로 빠르게 그래프가 우상향
- 2019년 12월에 주춤, 2020년 8월에 대폭 상승하는 모습을 보여줌
    - 연말 여파로 잠시 줄어들었다가 1월에 새해 마음가짐으로 인해 다시 상승하는 것으로 예상됨
    - 학생들이 많을 것으로 예상되는 플랫폼이므로 방학인 8월에 상승하는 모습을 보이지 않을까 예상됨
### Heatmap
>Check correlation btw human features
- 모든 컬럼 간에 아주 강한 상관관계를 보임
# 4. Modeling, Predict and Solve the problem<a name="modeling"></a>
## 4.1 Listing possible model<a name="modellisting"></a>
- 아래 2가지 모델을 중심으로 문제를 해결해보고자 한다.
    - Prophet
    - Sarimax
## 4.2 Modeling<a name="predicting"></a>
### 4.2.1 Modeling & Tuning & Fold
### 1) Prophet
- train_old 학습으로 train_new(test data)를 예측
- 공휴일 추가
    - 법정 공휴일 추가
    - 법정 공휴일 전일도 추가
    - 대회시작일, 종료일이 큰 이벤트였을 것으로 판단하여 추가
### Holiday 추가
- 법정 공휴일, 법정 공휴일 전일, 대회시작일, 대회마감일
### Parameter Tuning & Cross Validation
- for문으로 최적의 파라미터 탐색
- 대회평가방식인 rmse 사용
- prophet 내 cross validation 사용
    - best rmse score(in cross_val):  159.65631840326202
    - 최적의 파라미터:  {'changepoint_prior_scale': 0.01, 'seasonality_prior_scale': 0.015, 'holidays_prior_scale': 3}
### Model Fitting 및 시각화
- 최적의 파라미터로 모델을 학습시킴 (향후 30일 예측)
- 피팅된 컴포넌트를 시각화
    - 예측 : 예측을 벗어나는 데이터도 있으나, 전반적으로 잘 잡아준 모습
    - 트렌드 : 시작점부터 점점 증가하는 추세, 2019년 8-9월부터 가파르게 상승하는 경향
    - 주별 : 주 초에 데이터가 많이 몰리고, 주말에는 급격하게 감소하는 모습 (주말에는 활동이 적음)
    - 연별 : 연초에 증가, 연말에 감소하며 방학게 증가하고 학기 중에 감소하는 추세를 보여줌
        - 단, 페이지뷰의 경우 연초에만 조금 다른 추세를 보여줌. 연초에 사용자/세션/신규방문자가 늘어난 이후에 늘어나는 경향
### 2) SARIMAX model
- for문 이용하여 최적의 파라미터 탐색
- AIC로 최적값 선정
    - The smallest AIC is 14.0 for model SARIMAX(1, 0, 2)x(1, 1, 2, 12)
- 최적 파라미터로 모델학습 및 향후 30일 예측
### 4.2.3 Evaluation(Compare) with rmse
- 예측된 값을 바탕으로 모델평가
- 대회평가방식인 rmse 점수 확인
  - Prophet 모형의 경우 2.17
  - Sarimax 모형의 경우 3.53
- Prophet 모형이 훨씬 우수하게 나옴
### 4.2.4 Ensemble
- 두 모델의 앙상블을 통해 더 나은 예측력 보여주는지 확인
- 2.19로 Prophet 단독 예측력보다 못함을 확인
    - Prophet 단독 모델로 모델 확정
### 4.2.5 Fitting on best model with train data(train_old + train_new)
- train data에 모델 학습 및 향후 30일 예측
- 제출
  - 최종 예측값 : 5.83064 (20등 /453팀)