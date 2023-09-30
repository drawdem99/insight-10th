# 세션 6) 회귀 심화

## 1. 경사하강법(Gradient Descent)

- 모델은 '어떻게' 학습하는가? : cost function을 바탕으로 오차를 줄여가나는 방식으로 parameter가 업데이트
- "어느 방향으로 갈 것인가"를 결정

1. 원리
    - 손실 함수 그래프 위의 점을 기준으로(아래로 볼록한 그래프), 최소값과의 상호 위치에 따라 선 위의 점이 이동하는 방향이 다르다.
    - 최소값에 도달하면 미분값이 0이 되며 멈춤.
2. 학습률
    - 미분 값은 '방향'만을 결정한다.
    - 경사하강법의 '학습한다'라는 것은 '오차의 최솟값을 찾아나간다'라는 뜻
    - Learning rate : '보폭'의 개념. 너무 크다면 수렴하지 못하고 발산하게 되고, 너무 작다면 수렴할 때까지 오랜 시간이 걸림.
3. 경사하강법으로 계산해보기
    - cost function에서 결정해야 하는 것은 w1, w0 두 가지이다.
    - 1) 각각의 변수에 대해 편미분을 한다
    - 2) 해당 미분계수에 대해 각각의 변수를 업데이트
    - 위의 과정을 반복함에 따라 우리가 원하는 w1, w0 값에 수렴한다.
4. 경사하강법의 문제점
    - Local Minima 문제 : 모든 cost function이 하나의 최소값을 가지는 형태가 아니다. local minimum과 global minima 분리가 가능해야함
    - 적절한 step size를 찾지 못하는 문제 : 학습률을 어떻게 설정하느냐에 따라 시간이 오래걸리거나, 수렴하는 값을 아예 찾지 못할 수 있다
5. 해결법
    - 학습을 진행하며 학습률을 지속적으로 바꾸는 Adaptive Gradient Descent 이용
    - 모멘텀 : 기울기에 관성을 부여하여 작은 기울기는 쉽게 넘어가게 만듬 -> local minima를 탈출

## 2. 규제선형모델

- 모델이 훈련세트에 과대적합되지 않도록 '규제'를 가하고자 등장한 모델
- 규제 방식의 종류 : 회귀 계수의 크기를 제어해 과적합을 개선하려면 비용 함수의 목표는 아래와 같다.
    - 최적 모델을 위한 cost 함수 목표 = Min(학습데이터 잔차 오류 최소화 + 회귀계수 크기 제어)
    - 비용 함수 목표 = Min(RSS(W) + alpha * |W|)
- alpha가 0인 경우에는 W가 커도 두번째 항의 값이 0이 되어 비용 함수가 첫 번째 항과 동일하게 된다.
- alpha가 무한대인 경우 두 번째 항이 무한대가 되므로 W를 0으로 최소화해야한다.
- 규제(Regularization) : 비용 함수에 alpha값으로 패널티를 부여해 회귀 계수 값의 크기를 감소시켜 과적합을 개선하는 방식

- 규제 선형 모델의 종류:
    1) 릿지 회귀 : 계수를 제곱한 기준으로 규제를 적용(L2 규제)
    2) 라쏘 회귀 : 계수의 절댓값 기준으로 규제를 적용(L1 규제)
    3) 엘라스틱넷 회귀 : L2 규제와 L1 규제를 결합한 회귀
    
- 편향과 분산의 trade-off 관계
    - 과소적합 : 모델 복잡도를 높이고 더 많은 특성을 사용하거나 다양한 모델 시도
    - 과대적합 : 더 많은 데이터를 수집하거나 특성 선택/추출, 규제를 사용하여 모델 복잡도 줄임

## 3. Scaling

- 데이터 전처리 과정 중 하나로 모든 피쳐들의 데이터 분포/범위를 동일하게 조정하는 과정
1) 표준화(Standardization)
    - 입력된 값들의 정규 분포를 평균이 0이고 분산이 1인 표준 정규 분포로 변환
    - 데이터가 평균으로부터 얼마나 떨어져 있는지 나타냄
2) 정규화(Normalization)
    - 입력된 값들을 -과 1 사이의 값으로 변환해 다른 피쳐의 크기들을 통일
    - 모든 값이 0과 1 사이로 변환되기에 비교하기 용이함.
    
- 스케일링 변환 시 주의사항
    - 사이킷런 : sciPy와 Toolkit을 합친 파이썬 기반 머신러닝용 라이브러리
    - 학습데이터 : 모델이 학습하는데 사용되는 데이터
    - 테스트데이터 : 모델의 성능을 검증하는데 사용됨
- 스케일링 종류
    - StandardScaler() : 특성의 평균을 0, 분산을 1로 맞추는 표준화 수행
    - MinMaxScaler() : 모든 피쳐들이 0과 1사이의 데이터 값을 갖도록 변환
    - MaxAbsScaler() : 데이터가 -1과 1 사이에 위치하도록 스케일링
    - RobustScaler() : 데이터의 중앙값이 0, IQE(Q3-Q1)=1 이 되도록 스케일링
    - Normalizer() : 각 데이터 포인트의 norm을 1로 만들어주는 스케일링 / 각 행마다 정규화가 진행

## 4. 차원 축소

- 차원 : 수학에서 공간 내에 있는 점 등의 취치를 나타내기 위해 필요한 축의 개수
- 차원 축소 : 데이터에서 차원 = 변수의 수
- 차원이 크면 시각화가 어렵고, 이해/분석하기 어렵다 -> 차원을 줄이는 방법이 차원 축소

### 대표적인 차원 축소 알고리즘 (PCA, LDA, SVD, NMF)

- PCA(주성분 분석)
    - 목적 : 더욱 적은 수의 변수로 전체 변수의 분산을 설명 가능하다면 그 수를 줄이는 것이 가능
    - PCA : 고차원 데이터를 기존의 분산을 최대한 보존하는 선형 독립의 새로운 변수로 변환
    - 고유벡터 / 고유값 : 어떤 행렬에도 불구하고 방향성을 유지할 수 있는 벡터가 고유벡터.
- LDA(선형 판별 분석)
    - PCA처럼 입력 데이터 셋을 저차원 공간에 투영함으로서 차원을 축소
    - 특정 공간상에서 클래스 분리를 최대화하는 축을 찾기 위해 클래스 간 분산과 클래스 내부 분산의 비율을 최대화
- SVD(특이값 분해)
    - 정사각행렬이 아닌 곱의 형태의 다양한 행렬을 분해
- NMF(비음수 행렬 분해)
    - 하나의 객체정보를 음수를 포함하지 않은 두 부분 정보로 인수분해
    - 대량의 정보를 의미 특징과 의미 변수로 나누어 효율적으로 표현
    
### 차원 축소가 필요한 이유

1. 차원의 저주 : 차원이 커짐에 따라 공간의 범위가 기하급수적으로 증가해서 많은 sample이 필요해지는 현상
2. 과적합 문제 : 모델이 너무 정교하여 일반화 결과를 도출할 수 없는 상황
3. 분석 및 시각화 용이 : 복잡한 형태의 데이터 구조를 낮은 차원에서 분석하게 되면 구조를 파악하기 쉽거나 시각화하기 쉽다.