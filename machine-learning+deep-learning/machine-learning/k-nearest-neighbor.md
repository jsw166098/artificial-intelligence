# K 최근접 이웃을 통한 머신러닝 모델

## K 최근접 이웃

* 새로운 데이터에 대해 예측할 때 직선상 가장 가까운 거리에 위치한 데이터를 통해 판별한다.
* 주위의 데이터로 판별하고자 하는 데이터를 판단한다.
* 데이터가 많은 경우 모든 데이터 마다 거리를 계산하기 때문에 비효율적이다.

### Scikit-learn 라이브러리 이용

* KNeighborsClassifier(): k-최근접 이웃 분류 모델을 만드는 사이킷런 클래스
* fit(): 사이킷런 모델을 훈련할 때 사용되는 메서드
* predict(): 사이킷런 모델을 훈련하고 예측할 때 사용하는 메서드
* score(): 훈련된 사이킷런 모델의 성능을 측정한다. 정확도를 측정하며 먼저 predict() 메서드를 통해 예측을 한 후 정답과 비교하여 올바르게 예측한 개수의 비율을 반환한다. 

## K 최근접 이웃 모델을 통한 물고기 종 분류

### 도미, 빙어 데이터
~~~
//도미 데이터
bream_length = [25.4, 26.3, 26.5, 29.0, 29.0, 29.7, 29.7, 30.0, 30.0, 30.7, 31.0, 31.0, 
                31.5, 32.0, 32.0, 32.0, 33.0, 33.0, 33.5, 33.5, 34.0, 34.0, 34.5, 35.0, 
                35.0, 35.0, 35.0, 36.0, 36.0, 37.0, 38.5, 38.5, 39.5, 41.0, 41.0]
bream_weight = [242.0, 290.0, 340.0, 363.0, 430.0, 450.0, 500.0, 390.0, 450.0, 500.0, 475.0, 500.0, 
                500.0, 340.0, 600.0, 600.0, 700.0, 700.0, 610.0, 650.0, 575.0, 685.0, 620.0, 680.0, 
                700.0, 725.0, 720.0, 714.0, 850.0, 1000.0, 920.0, 955.0, 925.0, 975.0, 950.0]

//빙어 데이터
smelt_length = [9.8, 10.5, 10.6, 11.0, 11.2, 11.3, 11.8, 11.8, 12.0, 12.2, 
                12.4, 13.0, 14.3, 15.0]
smelt_weight = [6.7, 7.5, 7.0, 9.7, 9.8, 8.7, 10.0, 9.9, 9.8, 12.2, 13.4, 
                12.2, 19.7, 19.9]
~~~

### 산점도

* 빙어와 도미 모두 비례해서 늘어남
* 도미에 비해 빙어는 길이가 늘어나도 무게가 많이 늘지 않는다. -> 무게가 길이에 영향을 덜 받는다.

~~~
plt.scatter(bream_length, bream_weight)
plt.scatter(smelt_length, smelt_weight)
plt.xlabel('length')
plt.ylabel('weight')
plt.show()
~~~

### k-최근접 이웃 모델링

#### 1. 데이터 합치기

* 두 개의 리스트를 합친다.
~~~
weight = bream_length + smelt_length
length = bream_length + smelt_length
~~~

#### 2. zip() 함수를 통한 2차원 리스트 생성

* zip() 함수는 나열된 리스트 각각에서 하나씩 원소를 꺼내 반환한다.
* 사이킷런은 각 리스트의 특성을 세로로 늘어뜨린 2차원 리스트를 사용하게 된다.
~~~
fish_data = [[l, w] for l, w in zip(length, weight)]ㄹ
~~~

#### 3. 정답 데이터 생성 및 원 핫 인코딩

* 머신러닝에서 이진 분류의 경우 정답을 1, 그 외에는 0을 둔다. 이를 원 핫 인코딩이라고 한다.
* 정답 데이터 fish_target을 생성한다.
~~~
fish_target = [1]*35 + [0]*14
~~~

#### 4. 사이킷런 패키지에서 클래스 임포트 후 객체 생성

* 사이킷런 패키지에서 KNeighborsClassifier 클래스를 임포트한 후 객체를 생성한다.
~~~
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier()
~~~

#### 5. fit()을 통한 데이터 전달 및 훈련

~~~
kn.fit(fish_data, fish_target)
~~~

#### 6. score()를 통한 사이킷런 모델 평가

* 0~1 값을 반환하며 정확도를 측정한다.
~~~
kn.score(fisn_data, fish_target)
~~~

#### 7. predict()를 통한 새로운 데이터 예측

~~~
kn.predict([[30, 600]])
~~~

#### _fit_X, _y 속성

* _fit_X 속성은 fish_data 값들을 가지고 있다.
* _y 속성은 fish_target 값들을 가지고 있다.
~~~
kn._fit_X
kn._y
~~~

#### 참고 데이터 수

* 참고 하는 데이터의 수를 변경할 수 있다.
* 디폴트 값은 5이다.
* 다음의 경우 49개 데이터 모두 고려되며 디폴트 값인 5인 경우보다 정확도가 떨어진다.
~~~
kn49 = KNeighborsClassifier(n_neighbors = 49)
kn49.fit(fish_data, fish_target)
kn49.score(fish_data, fish_target)
print(35/49)
~~~


