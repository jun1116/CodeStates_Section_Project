# AI부트캠프_01_장형준_Section2_Project

# Project 기획, 분석배경

새로운 교통수단 패러다임으로, 개인형 이동수단을 지칭하는 Personal Mobility (이하 PM)이 떠오르고 있으며, 이는 주로 전기를 동력으로 하여 친환경적이고, 1~2인이 이용하는 교통수단을 지칭합니다. 

전동킥보드|전기자전거
:---:|:---: 
<img src='https://image.chosun.com/sitedata/image/201911/18/2019111801156_0.png' width='300px'/> | <img src='https://img7.yna.co.kr/etc/inner/KR/2019/07/19/AKR20190719014000057_01_i_P4.jpg' width='300px'/>
(아이나비 스포츠 로드 기어)|(카카오T 바이크) 
<img src='https://media.giphy.com/media/vBHN70eHOAIxZL4DKB/giphy.gif' width='300px'/>|<img src='https://media.giphy.com/media/3oKIPjagnSA6mZWS1G/giphy.gif' width='300px'/>

또한, 다수의 업체들이 지자체와의 협약 또는 자체적으로 특정 지역에서 (주로 시 단위에서) 공유자전거 및 공유전동킥보드 사업을 하고있습니다. 

이들은 대부분 반드시 지켜야하는 대여소가 정해져있지 않지만, 지역적 특성에 맞게 몇군데를 정하여 주로 관리가 이루어지며, 전동킥보드의 경우 대여량이 가장 적은 새벽시간대에 유지관리를 위한 차량이 돌아다니며 전기배터리를 교체하고, 간단한 수리 및 재배치가 이루어집니다. 

이미 대학가에서는 2~3업체가 경쟁하고 있으며, 관계법령도 만들어지고, 친환경 이동수단으로 인정받기 시작한 PM이, 자전거 도로 등의 인프라 구축과 함께 앞으로 더욱 활성화 될 것으로 예상됩니다. 

또한 2020년은 코로나로 인하여 사람들의 이동패턴에 다양한 변화가 생겼던 한 해이기도 합니다. 나중에도 코로나와 같은 질병으로 인하여 사람들의 행동패턴이 변화할 수 있다고 생각하며, 이와 관련하여, 대여소 근처 지역의 코로나 확진자 수가 어떤 영향을 미칠 수 있을지 함께 분석하고자 하였습니다. 

그리하여 수요에 맞는 차량 재배치가 매출에 큰 영향을 미칠 수 있는 요소라고 생각되어 이번 프로젝트를 진행하게 되었습니다. 

---

# 분석 목표

특정 대여소 또는 소규모 지역단위의 공유 PM 대여량 예측을 합니다. 

대부분의 업체들이 대여기록을 공개하고 있지 않기에, 그나마 공개된 2015년부터 서울특별시에서 활발히 운영되고있는 "따릉이"의 데이터를 [서울시 열린데이터광장]에서 구할 수 있었습니다. 공유되는 PM이라는 점에서 이번 프로젝트의 분석에 사용될 데이터로 가장 적합하다고 생각되어 위 데이터를 사용하였습니다. 

지역범위를 크게하면 해당 지역의 특성 (대학가, 주택가, 중심업무지구, 지하철역근처, 유흥가 등) 이 약해질 수 있으며, 이렇게 상향평준화된 대여량예측을 하며, 결국 어딘가의 대여소는 늘 부족한 상태이고, 다른 대여소는 늘 PM들이 놀고있는 상태가 될 수 있으므로, 지역범위를 축소하였으며, 축소는 임의로 진행하여 저에게 가장 익숙하고 지하철역 근처이며 유흥가이기도한 강남역 근처의 대여소 5곳으로 한정하였습니다. 

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5rovm%2FbtqXoa7xmcc%2FzQ0WXHITzoEegdQPySKQZK%2Fimg.png" width="400"/>  

전체 데이터
<img src="https://github.com/jun1116/CodeStates_Section_Project/blob/master/Section_2/images/Untitled%202.png?raw=true" width="400"/>  

대여소
<img src="https://github.com/jun1116/CodeStates_Section_Project/blob/master/Section_2/images/Untitled%201.png?raw=true" width="400"/>  

대여소번호|보관소명|자치구|상세주소|위도|경도
---|---|---|---|---|---
2407|역삼.서초.삼성 세무서 앞 (역삼빌딩 앞)|강남구|강남구 역삼동 804|37.498470|127.030113
2231|삼성타운(삼성생명) A동 맞은편|서초구|서울특별시 서초구 서초대로 405|37.496914|127.024963
2505|우성아파트사거리 (기업은행앞)|서초구|서초동 1374|37.493259|127.029533
2515|서초초등학교 후문|서초구|서울시 서초구 서운로 178|	37.498928|127.024727
2409|역삼동 디오슈페리움 (우성아파트 사거리)|강남구|강남구 역삼동 858(강남대로 342 앞 도로)|37.492062|127.030746

---

# 데이터 구축 (간략)

1. 서울시 공공데이터셋에서 공유자전거 대여기록 2년치 데이터 (약 2GB , 1800만 Samples)를 처리하여 각 시간대별 대여량 산출
2. 기상청에서 2019년과 2020년의 서울 기상정보 데이터를 처리하여 각 시간대별 기온,강수,풍속,습도,적설 데이터 산출
3. 대한민국 공휴일 정보를 통하여 2019년, 2020년 공휴일 데이터 산출
4. 서울시 구별 코로나 데이터를 통하여 2020년의 강남역 근처 반경 4km (이수~잠실~양재)의 일별 코로나 확진자수 산출
5. 서울시 지하철 호선별 역별 시간대별 승하차인원 데이터를 처리하여 2019,2020년 월별 시간대별 강남역의 승하차인원 산출

---

---

# 기초데이터 EDA및 시각화

## 1. 전체 데이터의 BoxPlot과 휴일과 평일의 차이

<img src='https://github.com/jun1116/CodeStates_Section_Project/blob/master/Section_2/images/Untitled%203.png?raw=true' width='500px'/>
전체 데이터의 평균은 5.79이며, 왼쪽은 boxplot입니다. 박스플롯에서는 각 퍼센타일의 위치를 보여주며 안에 보이는 파란색박스가 25~75%까지의 대부분의 데이터가 들어있는 영역이며, 그 안의 선으로 평균값 (5.79)가 , 박스의 위에 약 10정도가 75퍼센타일값, 1~3 사이에 25퍼센타일값이 위치합니다. 

<img src='https://github.com/jun1116/CodeStates_Section_Project/blob/master/Section_2/images/Untitled%204.png?raw=true' width='500px'/>
휴일과 평일의 대여량입니다. 

일반적인 토요일과 일요일 + 기준연도의 공휴일을 합쳐서 작성하였으며, 평일의 대여량이 더욱 많은것을 볼 수 있습니다. 

<img src='https://github.com/jun1116/CodeStates_Section_Project/blob/master/Section_2/images/Untitled%205.png?raw=true' width='500px'/>
19,20년도 통합 월별 대여량과 시간대별 대여량입니다. 

왼쪽은 봄이 시작하는 3월부터 대여량이 증가하며, 여름인 6,7,8에 조금 감소하고, 가을인 9,10,11월에 다시 증가하였다가 겨울에 감소하는 추세를 보여줍니다. 

오른쪽은 각 시간대별 대여량입니다. 오전의 출근시간대인 7~9시 보다 오후의 최근시간대인 17~19시가 훨씬 많은 사용량을 보여주고있습니다. 

이는 강남역 근처 대여소를 출근할때보다 퇴근할 때 더욱 많이 사용한다는 지역적 특색을 보여줍니다. 개인적인 추측으로는 출근하는데 정신없는 와중에 편히 버스타고 출근하려는 심리와, 퇴근길에는 일을 마치고 돌아가는길에 운동삼아서 자전거를 이용하려는 심리가 작용하지 않았을까? 라고 생각해봅니다.

## 2. 시간대별 지하철 승하차인원

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%205.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%205.png)

시간대별 지하철의 승하차인원입니다. 왼쪽은 승차인원이고, 오른쪽은 하차인원입니다. 

**강남역부근은 CBD (Central Business District, 중심업무지구)로 수많은 회사가 근처에 밀집되어있으며, 또한 음식점, 유흥업소 등 사람들의 만남이 이루어지는 장소**이기도 하여, 오전시간에 많이 사람들이 업무를 위해 몰려들고, 오후에는 만남을위해 몰려드는 위치적 특성을 갖고있습니다.

강남역의 승하차 인원을 시간대별로 그린 데이터를 보면 오전에는 승차인원이 적은반면 하차인원이 많고, 오후에는 승차인원과 하차인원 모두 많은 것을 볼 수 있습니다(하차의 경우 오전보다는 조금 적음).현재 **따릉이데이터는 강남역의 바로 근처에 있는 대여소의 대여기록**입니다.

> 이 상황에서 지하철 승차인원이 많다는것은, 많은 사람들이 지하철을 타러간다는 뜻이므로,회사->강남역 의 따릉이 이동이 형성될 것으로 추정되며,강남역에 돌아다니는 사람의 수가 줄어드는 이동을 나타내고

> 하차인원이 많다는 것은, 사람들이 지하철에서 나와 이동을 하는 상황이므로, 따릉이를 타고 회사로 가는상황이거나 강남역 부근에 돌아다니는 사람의 수가 많아지는 이동 대략적으로 이렇게 생각할 수 있습니다.

버스의 시간대별 승하차 기록을 함께 사용한다면 해당 시간대에 강남역에 머무는 사람을 알 수 있겠지만, 명확한 분석을 위해 서울시의 노선별 데이터와 경기도의 광역버스 데이터를 집계하는데에 더 큰 시간을 소요할 수 없기에 이 점이 아쉽습니다.

## 3. 월별 강남역 근처 구 (서초구,강남구) 코로나 확진자수

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%206.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%206.png)

강남역 부근의 월별 코로나 확진자 수 차트입니다. 1~7월까지는 비교적 적었으나, 8월에 급증하였고, 11월부터 다시 증가하여 12월에 최고점을 기록한 것을 볼 수 있습니다. 

# 문제 정의 (Problem Description)

직접 구축한 데이터를 통하여 모델 학습을 통해 미래의 어떠한 시간대에, 기상예보와 당일 코로나 확진자수의 에측 또는 추이와 해당 시간대의 지하철 승하차인원 데이터를 통하여 강남역 근처의 대여소에서 따릉이의 대여량을 예측합니다.

평가지표로 $R^2$를 사용합니다. $R^2$는 0~1 사이의 값을 가지며, $R^2$=0.5는 딱 평균만큼 예측한다는 것이고, 그보다 높을수록 예측을 잘 한다는 것을 의미합니다. 

## 데이터 전처리 (Feature Engineering)

이미 데이터를 구축할 때 전처리 진행을 끝마친 상태였으며 구축과정에 사용된 코드에 대해서 설명하기에 시간적으로 너무나 부족하여 넘어갑니다. 

구축 이후 전처리에서는 데이터 속 토요일 일요일 정보와, 공휴일정보를 합쳐서 그날이 쉬는날인지 아닌지를 한번에 나타내는 column을 추가하였습니다. 

# 모델링과 하이퍼파라미터 튜닝 (Modeling with Parameter Tunning)

## 렌덤포레스트 (RandomForestRegressor)

모델의 기본성능부터 월등하여, 파라미터 튜닝결과 $R^2$의 최대값이 약 0.768를 기록하였습니다. 

```python
from sklearn.model_selection import RandomizedSearchCV
rf_params = {
    'n_estimators':[50,500],
    'max_features':['auto','sqrt'],
    'max_depth':[5,15,20],
    'min_samples_split':[5,10]
}
rand_rf = RandomizedSearchCV(estimator=RandomForestRegressor(),
                             param_distributions = rf_params,
                             n_iter=10,
                             cv=5,
                             verbose=2,
                             random_state=42,
                             n_jobs=-1,
                             scoring='neg_mean_squared_error'
                             )
rand_rf.fit(X_train,y_train)
rand_rf_pred = rand_rf.predict(X_val)
print("MSE : ",mean_squared_error(y_val,rand_rf_pred))
print("RMSLE : ",rmsle(y_val,rand_rf_pred))
print("R2 : ",r2_score(y_val,rand_rf_pred))
```

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%207.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%207.png)

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%208.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%208.png)

랜덤포레스트 모델에서 중요시하는 특징은 승차데이터와 기온, 하차, 습도 순이었습니다. 

## XGBoost

강력한 성능을 보여주며 대회에도 자주 등장하는 XGBoost를 사용한 모델에서는 하이퍼파라미터 튜닝을 하기에 워낙 많은 파라미터가 존재하여 다수의 파라미터를 건드리기 쉽지않았습니다. 간략하게 결과로는 $R^2$값 0.767을 기록하였습니다

```python
xgbr = XGBRegressor(n_jobs = -1, random_state=42,learning_rate=0.0811, metric=rmsle, n_estimators=5000, refit=True)
eval_sets=[(X_train,y_train),(X_val , y_val)]
xgbr.fit(X_train, y_train,
         eval_set = eval_sets,
         early_stopping_rounds = 70,
         verbose=50)

xgbr_pred = xgbr.predict(X_val)
print("MSE : ",mean_squared_error(y_val,xgbr_pred))
print("RMSLE : ",rmsle(y_val,xgbr_pred))
print("R2 : ",r2_score(y_val,xgbr_pred))
```

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%209.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%209.png)

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2010.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2010.png)

XGBoost 모델에서의 특징 순위 또한, 승차, 기온, 하차 순이었으며, 그 다음으로 휴일여부였습니다.

## LightGBM

XGBoost와 더불어 가볍고 빠르며, 성능또한 뛰어난 LightGBM모델을 사용하여 에측하였습니다. 결과로는 $R^2$값 0.782를 기록하였습니다.,

```python
lgb_model=LGBMRegressor()
eval_set = [(X_train,y_train),(X_val,y_val)]
lgb_model = LGBMRegressor(metric='rmse',n_jobs=-1,random_state=42,
                          n_estimators=5000, refit=True,
                          learning_rate=0.0855, 
                          reg_lambda = 1)
lgb_model.fit(X_train, y_train, eval_set = eval_set , early_stopping_rounds = 70, verbose=50)
lgb_pred = lgb_model.predict(X_val)
print("MSE : ",mean_squared_error(y_val,lgb_pred))
print("RMSLE : ",rmsle(y_val,lgb_pred))
print("R2 : ",r2_score(y_val,lgb_pred))
```

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2011.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2011.png)

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2012.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2012.png)

eli5 라이브러리를 통한 permutation Importance(무작위에러값측정) 결과 → 승차의 변화에 Error가 가장 컸다

LightGBM 모델 또한 특징의 순위 1등은 승차였으며, 이후 하차, 기온, 휴일여부가 뒤따랐습니다. 

## Feature Importance

### pdpbox

- 코드

    ```python
    # !pip install pdpbox
    from pdpbox.pdp import pdp_isolate, pdp_plot
    def pdpPlotting2d(model, feature, data):
        isolated = pdp_isolate(
            model = model,
            dataset = data,
            model_features = data.columns,
            feature = feature,
            grid_type = 'percentile',
            num_grid_points = 10
        )
        pdp_plot(isolated , feature_name = feature, plot_params={'font_family':font_name,
                                                                 'dpi':120})

    pdpPlotting2d(lgb_model, '승차', X_val)
    ```

승차량

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2013.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2013.png)

기온

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2014.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2014.png)

강수량

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2015.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2015.png)

코로나 확진자수

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2016.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2016.png)

pdpbox라이브러리를 활용하여, 가장 좋은 성능을 보였던 LightGBM 모델의 승차량과 강수량에 대한 그래프입니다. 

pdpbox에서는 특성값에 따라 타겟값(대여량)의 추이를 볼 수 있습니다. 

승차의 경우 승차의 증가 == 대여량의 증가 를 보이며, 강수량의 경우 강수량 증가 == 대여량 감소 를 보입니다. 

기온의 경우 일반적인 생각대로 야외활동하기 좋은 온도인 10도 ~ 25도까지 증가하며, 그 이후 더위로 인하여 감소합니다. 

코로나 확진자수의 경우 증가할수록 대여량이 감소하는 모양새를 보여줍니다. 

### shap

shap 라이브러리를 통해서는 실제 모델이 어떤 방식으로 예측하는지 볼 수 있습니다. 

- 코드

    ```python
    # !pip install shap
    import shap
    row=5
    row = X_val.iloc[[9]]
    explainer = shap.TreeExplainer(lgb_model)
    shap_values = explainer.shap_values(row)
    print(y_val.iloc[9])

    shap.initjs()
    shap.force_plot(
        base_value=explainer.expected_value, 
        shap_values=shap_values,
        features=row
    )
    ```

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2017.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2017.png)

![AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2018.png](AI%E1%84%87%E1%85%AE%E1%84%90%E1%85%B3%E1%84%8F%E1%85%A2%E1%86%B7%E1%84%91%E1%85%B3_01_%E1%84%8C%E1%85%A1%E1%86%BC%E1%84%92%E1%85%A7%E1%86%BC%E1%84%8C%E1%85%AE%E1%86%AB_Section2_Project%20c3fec14cc06b442cb6d0803bc0facac5/Untitled%2018.png)

실제값 6인 두 데이터의 예측을 보면, 승차, 기온, 휴일여부, 코로나 확진자수 등 다양한 데이터의 값들이 모여서 어떻게 예측을 보여주는지 간단하게나마 볼 수 있습니다. 

승차값에 따른 예측량 차이 , 기온에 따라 예측량 등을 볼 수 있습니다. 

---

---

# 결과 요약

Personal Mobility의 공유사업에서 필수불가결한 대여량 예측을 지역적 범위를 줄여서 분석하였습니다. 

각 지역마다 다른 특성을 보여줄 수 있으며, 이번 프로젝트에서 분석한 강남역의 경우 Business와 만남 등의 활동이 주로 이루어지는 지역입니다. 

데이터 구축을 위해 공공데이터셋에서의 raw 파일을 받아서 직접 변환작업을 하였으며, 일반 시민들의 활동성에 영향을 줄 수 있는 요소로 짐작되는 데이터들을 함께 모아 작업하였습니다. 또한 2020년에 대유행한 코로나데이터를 통하여, 향후 전염병의 변화에 대하여 함께 분석하였습니다. 

사람이 이용하다보니 실제 유동인구에 대하여 대여량의 변화가 가장 컸으며, 기온, 시간대, 강수가 영향을 줍니다. 하지만, 코로나에 의한 영향도는 심각한 수준은 아니었습니다.

향후 대여량의 분석에 있어서도 지역적 특색을 확실히 인지하고 넘어가야함을 느꼈으며, 각 지역의 대여소 근처에 대한 분석이 상당히 중요함을 알 수 있었습니다. 

따라서, 현재 대여소단위로 운영하고 있지 않은 전동킥보드 대여사업의 경우 랜드마크 단위의 대여량 관리를 통하여, 해당 랜드마크(oo대학 , oo역, oo쇼핑몰, oo아파트단지) 를 중심으로 근처의 유동인구를 예측할 수 있는 대중교통 데이터와 날씨, 전염병 등의 데이터를 통하여 대여량을 예측할 수 있음을 보여줍니다. 

![https://media.giphy.com/media/nUSgpi4vHWqDC/giphy.gif](https://media.giphy.com/media/nUSgpi4vHWqDC/giphy.gif)
