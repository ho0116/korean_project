# 👣캐시워크 앱 리뷰 감성 분석
![image](https://github.com/ho0116/korean_project/assets/85285367/e723a370-5575-4839-a58a-782775311537)
KOELECTRA를 이용하여 Google Play Store의 캐시워크 앱 리뷰 긍부정을 예측해보는 프로젝트

## 1. 개요
### 1.1 문제 정의
만보기는 1965년 일본에서 개발된 걸음 수를 측정할 수 있는 기계로, 건강 증진을 위해 하루에 1만 보를 걷자는 공익 운동과 함께 시작되었다. 이 운동은 전 세계로 확산되어 현재까지도 꾸준한 인기를 끌고 있다. 스마트폰과 스마트 워치 등의 기술 발전으로 만보기 기능은 건강 관련 앱에 자연스럽게 통합되어, 많은 사람들이 Google Play Store에서 다운받아서 이용하고 있다. <a href="https://blog.socialmkt.co.kr/558">[1]</a>  
<div align="center"><img src="https://github.com/ho0116/2023pj_spotify/assets/85285367/92ab9528-3faf-47fd-8ea1-a2db2a80d62d" width="500"></div>

Google Play Store는 구글의 디지털 콘텐츠 서비스로, 안드로이드 기기에서 이용 가능하며 2023년 4월 기준 약 68.6%으로 많은 사람들이 이용한다. <a href="https://gs.statcounter.com/os-market-share/mobile/worldwide/">[2]</a> 사용자들은 앱을 다운받을 때 다른 사용자들의 평점과 리뷰를 주요 기준으로 삼아 결정한다. <a href="https://android-developers.googleblog.com/2021/08/making-ratings-and-reviews-better-for.html">[3]</a><br>

<div align="left"><img src="https://github.com/ho0116/korean_project/assets/85285367/2dec9f82-7872-4a97-b8d2-878a19fd97fe"></div>
캐시워크는 Google Play Store를 통해 다운로드할 수 있으며, 만보기 앱테크 중에서도 가장 일찍 또 널리 알려진 앱 중 하나이다. 아이지에이웍스가 자사의 데이터 분석 솔루션 모바일인덱스 고유의 알고리즘을 통해 아이지에이웍스 마케팅클라우드 <갓생 탐구생활> 리포트를 발표했다. 분석 기간은 2021년 1월 1일부터 2023년 3월 5일까지로 일평균 4000만 모바일 기기의 20억건 데이터(안드로이드, iOS 통합 데이터 기준)를 AI 알고리즘에 기반하여 분석했다. 2월 기준 사용자 수(MAU)와 2023년 3월 2일 기준 앱 일간 사용률(DAU)을 집계한 결과에 따르면, 리워드앱 캐시워크가 가장 많다는 것을 알 수 있다. <a href="https://www.madtimes.org/news/articleView.html?idxno=17018">[4]</a> 캐시워크는 걷기 운동을 통해 소비된 칼로리, 움직인 거리와 시간까지 계산해주고 걸으면 걸을수록 포인트가 쌓이며, 이렇게 쌓은 포인트는 앱 내 숍에서 편의점이나 카페와 같은 다양한 쿠폰을 구입할 수 있다. 사용자들은 건강을 챙기면서 동시에 혜택을 누릴 수 있어 많은 사람들에게 사랑받고 있는 앱이다. <a href="https://www.mk.co.kr/news/culture/10252024">[5]</a> <br> 
캐시워크는 사용자들이 어떤 기능이나 서비스를 좋아하는지, 어떤 부분에서 경쟁사와 차별화되는지 등을 파악할 수 있으며, 사용자들이 원하는 기능을 강화하고 경쟁사와 차별화된 서비스를 제공할 수 있다. 이를 통해 리워드 앱에서의 경쟁력을 강화할 수 있다. 또한 사용자들의 리뷰를 통해 캐시워크의 브랜드 이미지와 고객 인식이 형성될 수 있다. 긍정적인 리뷰는 캐시워크의 이미지를 향상시키고 브랜드에 대한 고객들의 신뢰를 증대시킬 수 있다. 부정적인 리뷰는 캐시워크가 개선해야 할 부분을 도출하고 사용자들에게 더 나은 서비스를 제공하기 위한 노력을 보여줄 수 있다.

### 1.2 데이터 및 모델 개요
데이터는 github에 있는 크롤링한 건강관리 앱 리뷰 데이터에서 캐시워크 앱 리뷰만 활용<a href="https://github.com/park-gb/mHealthApp-review-textmining/">[6]</a>하여, 총 약 8만8천 건의 데이터에 대해서 사전 학습 언어 모델의 재학습(fine-tuning)을 수행한다. 

| 입력       |모델|출력|
|----------|---|---|
| 캐시워크 앱 리뷰 문장 |KOELECTRA small[3]|부정(0), 긍정(1)|

캐시워크 앱 리뷰 문장은 전처리를 통해서 가공한다. 대표적인 전처리는 결측치, 중복값을 제거한다.

## 2. 데이터
### 2.1 데이터 소스
GitHub <a href="https://github.com/park-gb/mHealthApp-review-textmining/blob/master/data/dataset_raw.xlsx">mHealthApp-review-textmining</a>  
건강관리 앱 424개에서 리뷰 54만 건을 수집한 데이터중에서 캐시워크 앱 리뷰만 사용한다. 

### 2.2 탐색적 데이터 분석
- 데이터명

|app|review|rating|
|:---:|:---:|:---:|
|앱이름|리뷰|평점|

- 데이터

||app|review|rating|
|---|---|---|---|
|0|캐시워크 - 적립형 만보기 첫화면|한번 업데이트 한뒤로 저희소속에서 다른사람들이 글 ...|3|
|1|캐시워크 - 적립형 만보기 첫화면|정말 다 좋은데 중간중간 걸음이 인식이 않돼요 그리고...|4|
|2|캐시워크 - 적립형 만보기 첫화면|캐시적립 후 캐시로 쿠폰 구매시 불법사용자로 의심...|1|
|...|...|...|...|
|88881|캐시워크 - 적립형 만보기 첫화면|대박..걸으면 포인트가 쌓인다는게 완전좋네...|5|
|88882|캐시워크 - 적립형 만보기 첫화면|이 앱 진짜 좋네요. 건강도 생각하고 포인트...|5|
|88883|캐시워크 - 적립형 만보기 첫화면|재밌는 어플이네요~~|5|

약 8만8천개의 리뷰가 있으며 1점부터 5점으로 구성되어 있다.
<div align="left"><img src="https://github.com/ho0116/korean_project/assets/85285367/9fe574d8-d1b1-490a-8df1-1d593d394295" width="500"></div>
1점은 14726개, 2점은 5722개, 3점은 10731개, 4점은 13966개, 5점은 43739개의 데이터로 구성되어 있다,  

### 2.3 데이터 전처리

원본 데이터에서 리뷰와 평점을 추출하고 평점에서 3점을 제외하고, 1점과 2점은 부정(0)으로, 4점과 5점은 긍정(1)으로 분류하여 78,153개가 되었다. 먼저 마침표가 2개이상(ex : 하...작가님)은 공백으로 대체 후, 한글 긍부정 예측이므로 한글이 없는 행은 필요가 없다고 판단하여 한글불포함행을 제외하니 1,095개가 제거되었다. 중복값 13,197개를 제거했고 결측치는 없었다. Google Play Store의 리뷰가 500자까지만 허용되고 15자 미만의 짧은 리뷰는 매우 제한적이며 정보가 부족하다고 판단하여 총 17,551개를 제거했다. 최종적으로 남은 데이터는 <b>46,310</b>개가 되었다.  



- 데이터

||review|label|
|---|---|---|
|0|정말 다 좋은데 중간중간 걸음이 인식이 않돼요 그리고...|1|
|1|캐시적립 후 캐시로 쿠폰 구매시 불법사용자로 의심...|0|
|2|이거 왜 뽑기 안 들어가요? 제 폰이 구데긴가요?|0|
|...|...|...|
|46307|걸음을 바로볼수있어서 너무좋아요. 포인트도 바로...|1|
|46308|대박 걸으면 포인트가 쌓인다는게 완전좋네요~걸으며...|1|
|46309|이 앱 진짜 좋네요. 건강도 생각하고 포인트도 얻고...|1|

46310개의 데이터가 있으며 긍정과 부정에 대한 그래프와 리뷰 문장 길이 분포도를 그려봤다.
<div align="left"><img src="https://github.com/ho0116/korean_project/assets/85285367/067b7b3e-310e-4185-8d13-b3420f56c2c1" width="500"><img src="https://github.com/ho0116/korean_project/assets/85285367/788507d5-9b05-4b67-bfdc-3869a026b5c5" width="500"></div>
데이터는 약 1:1.7의 비율로 나누어져 있으며, 긍정 데이터는 29,310개가 있고, 부정 데이터는 17,000개로 구성되어 있다. 리뷰는 50자 이하에 많이 몰려있다는 것을 알 수 있다.  

### 2.4 학습 데이터
- 1:1 비율(랜덤으로 부정 1500개, 긍정 1500개 총 3000개 추출)

|학습 데이터|검증 데이터|
|:---:|:---:|
|2400개|600개| 

||review|label|
|---|---|---|
|0|당첨확률두배라면서당첨이않되요.그래서91캐시만날라갔어...|0|
|1|얼마전까지만 해도 편의점이 있었는데 없어져서 쓸 이유가...|0|
|2|아니;;; 돈도 잘벌어지거 좋아요 좋은데 하루 100캐시 이상...|0|
|...|...|...|
|2997|진짜 짱이에요 근데 카카오페이지 캐시를 쇼핑으로 추가...|1|
|2998|다 좋은데 오늘 갑자기 상자에 70코인 정도가 없어졌어...|1|
|2999|정말 편하게 할 수 있도록 수정하신 것 같아요!!|1|


- 1:1.7 비율(랜덤으로 부정 1500개, 긍정 2550개 총 4050개 추출)

|학습 데이터|검증 데이터|
|:---:|:---:|
|3240개|810개|

||review|label|
|---|---|---|
|0|당첨확률두배라면서당첨이않되요.그래서91캐시만날라갔어...|0|
|1|얼마전까지만 해도 편의점이 있었는데 없어져서 쓸 이유가...|0|
|2|아니;;; 돈도 잘벌어지거 좋아요 좋은데 하루 100캐시 이상...|0|
|...|...|...|
|4048|좋은데 뽑기로 돈만 엄청 많이 버렸어요ㅠㅠ|1|
|4048|짱좋아요! 최고에여! 먹을것도 먹을수있어요!|1|
|4049|설치후 스마트폰 켜면 보이니 10,000보를 매일 걸을려...|1|

## 3. 재학습 결과
### 3.1 개발 환경
<img src="https://img.shields.io/badge/pycharm-000000?style=flat-square&logo=pycharm&logoColor=white"/> <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/> <img src="https://img.shields.io/badge/torch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white"/> <img src="https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white"/> <img src="https://img.shields.io/badge/numpy-013243?style=flat-square&logo=numpy&logoColor=white"/> <img src="https://img.shields.io/badge/transformers-81c147?style=flat-square&logo=transformers&logoColor=white"/> <img src="https://img.shields.io/badge/scikit-learn-F7931E?style=flat-square&logo=scikit-learn&logoColor=white"/>
### 3.2 KOELECTRA fine-tuning
KOELECTRA 모델을 fine-tuning하는 과정에서 두 가지 다른 데이터 세트를 사용했다. 첫 번째 세트는 1:1 비율로 구성된 3000개의 데이터이며, 두 번째 세트는 1:1.7 비율로 구성된 4050개의 데이터이다. 데이터를 학습시킨 후, 각 모델을 전체 데이터인 46310개에 적용하여 모델을 테스트했다.
### 3.3 학습 결과 그래프
<div align="left"><img src="https://github.com/ho0116/korean_project/assets/85285367/fc12a17d-a262-4b10-9b22-65d6757c346e"></div>  

<table>
  <tr align="center"><th></th><th></th><th>Epoch 1</th><th>Epoch 2</th><th>Epoch 3</th><th>Epoch 4</th></tr>
  <tr align="center"><th rowspan="2">1:1 비율</th><td>평균 학습 오차</td><td>약 0.54</td><td>약 0.42</td><td>약 0.37</td><td>약 0.30</td></tr>
  <tr align="center"><td>검증 정확도</td><td>약 0.81</td><td>약 0.79</td><td>약 0.84</td><td>약 0.85</td></tr>
  <tr align="center"><th rowspan="2">1:1.7 비율</th><td>평균 학습 오차</td><td>약 0.52</td><td>약 0.38</td><td>약 0.33</td><td>약 0.26</td></tr>
  <tr align="center"><td>검증 정확도</td><td>약 0.83</td><td>약 0.84</td><td>약 0.84</td><td>약 0.85</td></tr>
</table>
1:1비율의 학습 데이터는 학습할수록 평균 학습 오차는 감소하고 있고 검증 정확도는 Epoch 2에서 감소했다가 다시 증가했다. 1:1.7비율의 학습 데이터는 학습할수록 평균 학습 오차는 감소하고 있고 검증 정확도는 학습할수록 증가했다.


### 3.4 모델 적용

- 1:1 비율 모델을 전체 데이터(46310개)에 적용한 결과
```
test steps :  1 Accuracy :  0.875
test steps :  2 Accuracy :  0.875
test steps :  3 Accuracy :  0.75
...
test steps :  5787 Accuracy :  0.875
test steps :  5788 Accuracy :  0.875
test steps :  5789 Accuracy :  1.0
Accuracy: 0.84
test took: 1:38:04
```
1:1 비율을 학습한 모델을 적용하니까 Accuracy가 0.84가 나왔다.  


- 1:1.7 비율 모델을 전체 데이터(46310개)에 적용한 결과
```
test steps :  1 Accuracy :  0.875
test steps :  2 Accuracy :  0.875
test steps :  3 Accuracy :  0.625
...
test steps :  5787 Accuracy :  0.875
test steps :  5788 Accuracy :  0.75
test steps :  5789 Accuracy :  1.0
Accuracy: 0.85
test took: 1:32:31
```
1:1.7 비율을 학습한 모델을 적용하니까 Accuracy가 0.85가 나왔다.  
1:1 비율과 1:1.7 비율로 학습시킨 두 모델을 전체 데이터에 적용해 보았을 때, 예상과 달리 정확도에서 큰 차이가 나타나지 않고, 근소한 차이만을 보였다.

## 4. 배운점
원하는 데이터를 찾고 전처리하는 과정이 약간 어려웠다. 특히, 전처리 방법에 따라 모델의 성능이 크게 달라질 수 있다는 것을 알게 되었다.
긍정과 부정은 1:1.7 비율로 되어있어서 학습 데이터를 1:1과 1:1.7 비율로 학습시키면 정확도에서 많은 차이가 있을 거라고 생각을 했지만, 결과는 비슷하게 나왔다는 것을 보고 전처리가 잘 되면 상관이 없다는 것을 알게 되었다.
맞춤법과 띄어쓰기 교정을 했더라면 Accuracy가 더 높게 나오지 않았을까 하는 생각이 든다.
