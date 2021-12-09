# Viral Index(바이럴 지수)
**부제 : 입소문이 영상 콘텐츠의 흥행에 미치는 영향**   
### 바이럴 지수란?
SNS 플랫폼에서의 관련 컨텐츠로 인한 입소문으로 영화의 관객수에 미치는 영향을 점수로 표현한 것

## 🙋 Contributors
|이름|Github|
|---|---|
|김유진|[@youjin2github](https://github.com/youjin2github)
|박태남|[@Park-taenam](https://github.com/Park-taenam)
|신은식|[@Eunsik-Shin](https://github.com/Eunsik-Shin)
|이은성|[@eun-seong](https://github.com/eun-seong)
|이희원|[@Honi-huiwon](https://github.com/Honi-huiwon)

## ✍️ Projects
### Background
* SNS가 발달하면서 입소문으로 특정 컨텐츠가 홍보되는 현상이 많이 발생하고 있다.
* 이러한 입소문이 영상 제작물의 흥행에 영향을 주는 주요한 요인들 중 하나이다.
* 바이럴 지수가 영상 컨텐츠뿐만 아니라 다방면으로 사회에 영향을 끼치고 있다.

### Expected effects
* 바이럴 지수가 마케팅이나 사회 영향력 등 다양한 부분에서 활용될 수 있을 것이다.


### Process
1. [데이터 수집](https://github.com/movie-catcher/youtube-data-scraping)
   1. KOBIS 영화 기본 정보 스크래핑
   2. 영화와 관련된 유튜브 영상 정보 수집
2. 데이터 정제
    * [댓글 데이터 감성 분석](https://github.com/movie-catcher/youtube-data-scraping#-%EA%B0%90%EC%84%B1-%EB%B6%84%EC%84%9D)
    * [transformation](https://github.com/movie-catcher/data-analysis#data-transformation)
3. [EDA](https://github.com/movie-catcher/data-analysis#-eda)
   * 데이터 파악
   * 분산 안정화
   * 피쳐 셀렉션
4. [모델링](https://github.com/movie-catcher/modeling)
   * RandomForest 모델 사용
   * 영화 관련 Model1 과 입소문 관련 Model2로 구성


## 💾 Data

|data|desc|
|---|---|
|영화 관련 데이터|17~20년도 누적 관람객수 상위 50개의 영화(KOBIS)|
|OTT 관련 데이터|오징어 게임, 보건교사 안은영, 옥자, 승리호, 킹덤|

### Features

> **Movie Features**

|name|desc|
|:---:|---|
|영화명|영화 이름|
|개봉일|영화의 개봉일|
|매출액|영화의 총 매출액|
|**관람객 수**|영화의 누적 관람객 수|
|**제작 국가**|영화의 제작 국가|
|**배급사**|영화의 배급사 리스트, 여러 개일 경우 대표 배급사 하나만 선정|
|영화 코드|KOBIS 내의 영화 코드|
|**장르**|영화의 장르|
|영화종류|일반 영화 or 독립영화, 일반 영화가 독보적이라 제외|
|스케일|장편 or 단편, 장편이 독보적이라 제외|
|**러닝타임**|영화의 러닝 타임|
|**연령제한**|영화의 연령 제한|
|주연 배우|영화의 주연 배우 리스트|

*굵은 글씨: 실제 모델링에 사용된 피쳐


> **Viral Features**

|name|desc|
|:---:|---|
|조회수 평균|영화당 10개의 유튜브 영상 조회수의 평균|
|조회수 합|영화당 10개의 유튜브 영상 조회수의 합|
|댓글 수|해당 영화의 유튜브 영상 댓글 수의 총합|
|감성분석 점수|영화의 댓글들을 감성 분석한 결과의 절댓값 평균|
|배우 영향력|영화 개봉일 기준 주연배우 2명이 이전 3년간 주연으로 출연한 영화의 누적 관람객수 평균| 

## 🧩 Modeling
> Model1 -> error -> Model2 -> Viral index

* 최대한 **입소문**에만 영향을 줄 수 있는 피쳐들로 Viral Index를 구상하기 위해서 **잔차**를 이용
* 첫 번째 모델의 잔차를 사용하여 두 번째 모델링을 진행하였기 때문에,   
    영화와 직접적으로 관련된 피쳐들의 영향력은  두 번째 모델에서 예측을 진행
제거된 후
### Model1
1. movie feature로 모델링
2. 예측값(`y_hat`) 산출

### Model2
1. Model1에서 구한 예측값으로 잔차(`y`-`y_hat`) 계산
2. 잔차를 종속변수로 사용하여 viral feature로 모델링
3. 산출한 값을 scaling하여 Viral Index 산출

## 💡 Conclusion

2021년 가장 흥행했던 '오징어 게임'의 바이럴 지수를 100점 기준으로 삼아 다른 OTT 컨텐츠의 점수를 산정

|순위|OTT 컨텐츠 이름|Viral Index|
|:---:|:---:|---:|
|1|오징어 게임|100.00|
|2|보건교사 안은영|50.32|
|3|킹덤|50.22|
|4|승리호|37.93|
|5|옥자|11.94|

