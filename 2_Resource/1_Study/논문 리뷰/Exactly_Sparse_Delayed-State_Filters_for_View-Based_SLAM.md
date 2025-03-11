---
tags: #Study/springer journal, SLAM
aliases: 
-"loop closure method"
-"algorithm"
-"visual SLAM"
---
### **특징 기반 SLAM(Feature-based SLAM)의 희소성 문제**

#### **1. 특징 기반 SLAM 개요**

특징 기반 SLAM은 환경에서 추출된 특징(Feature)을 기반으로 로봇의 위치를 추정하고 맵을 구축하는 기법이다. 주로 사용되는 특징은 **점(코너, 엣지), 선, 평면** 등의 지형지물이며, 카메라, LiDAR, 혹은 IMU 등의 센서를 활용하여 특징을 감지한다.

대표적인 알고리즘으로는 **ORB-SLAM, PTAM(Parallel Tracking and Mapping), VINS-Mono** 등이 있으며, 주로 **비주얼 SLAM(Visual SLAM)** 방식에서 특징 기반 방법이 많이 활용된다.

---

### **2. 희소성 문제(Sparsity Issue)란?**

특징 기반 SLAM에서 **희소성(Sparsity)** 문제란, 환경에서 특징을 충분히 추출할 수 없는 경우 발생하는 문제를 의미한다. 즉, SLAM이 안정적으로 수행되기 위해서는 일정 수준 이상의 특징점이 필요하지만, 특징이 부족한 경우 알고리즘의 성능이 저하될 수 있다.

희소성 문제는 크게 다음과 같은 상황에서 발생한다.

#### **(1) 환경적 요인**

1. **단조로운 환경(Uniform Texture)**
    
    - 벽, 바닥, 천장 등과 같은 단조로운 표면에서는 특징을 추출하기 어렵다.
    - 예: 흰 벽이 있는 실내 공간, 눈 덮인 지역, 수중 환경 등
2. **반사 및 투명한 표면(Reflective & Transparent Surfaces)**
    
    - 유리, 거울, 반사 표면에서는 카메라 또는 LiDAR 센서가 정확한 특징을 추출하지 못함.
    - 예: 창문이 많은 실내 환경, 광택이 강한 바닥
3. **조명 변화 및 저조도 환경(Lighting Variability & Low-Light Conditions)**
    
    - 카메라는 조명 변화에 취약하며, 어두운 환경에서는 특징을 감지하기 어려움.
    - 예: 야간 주행, 터널 내부, 지하 공간

#### **(2) 센서 및 알고리즘 요인**

1. **센서 해상도 및 감지 한계**
    
    - 저해상도 카메라는 세밀한 특징을 감지하기 어려우며, LiDAR의 경우 일정 거리 이상에서는 특징이 부족해질 수 있음.
2. **매칭 에러(Feature Mismatch)**
    
    - 특징이 희소한 경우, 매칭이 제대로 되지 않아 오차가 누적될 가능성이 큼.
3. **프레임 간 변화 부족(Motion Blur & Small Displacement)**
    
    - 카메라 기반 SLAM의 경우, 카메라가 너무 빠르게 움직이면 모션 블러(Motion Blur)로 인해 특징이 검출되지 않을 수 있음.
    - 로봇이 너무 느리게 움직이면 연속적인 프레임 간 차이가 작아 특징 매칭이 어려워질 수 있음.

---

### **3. 희소성 문제의 영향**

1. **로컬라이제이션(Localization) 오류 증가**
    
    - 특징점이 충분하지 않으면 위치 추정(Localization) 오차가 커지고, 드리프트(Drift) 현상이 발생할 가능성이 높아짐.
2. **맵 품질 저하**
    
    - 맵을 구축할 때 특징이 부족하면 지도에 포함되는 정보가 적어지고, 전체적인 정확도가 떨어질 수 있음.
3. **루프 클로징(Loop Closing) 실패**
    
    - 루프 클로징(과거 방문한 지점을 재인식하는 과정)이 원활하지 않으면 누적 오차로 인해 전역 맵이 왜곡될 가능성이 있음.

---

### **4. 희소성 문제 해결 방법**

#### **(1) 센서 융합(Sensor Fusion)**

- **IMU + 카메라(VIO, Visual-Inertial Odometry)**: IMU 데이터를 활용해 특징이 부족한 구간에서도 위치를 예측할 수 있음.
- **LiDAR + 카메라**: LiDAR를 활용해 특징이 적은 환경에서도 깊이 정보를 보완할 수 있음.

#### **(2) 특징 개선(Feature Enhancement)**

- **딥러닝 기반 특징 검출(Deep Learning Feature Extraction)**
    - SIFT, ORB 등의 전통적인 특징점 대신, CNN 기반의 특징 추출 기법(D2-Net, SuperPoint) 사용.
- **광원 보정 및 HDR 처리**
    - 조명이 변하는 환경에서 안정적으로 특징을 검출하도록 보정.

#### **(3) 멀티-뷰 접근(Multi-View & Keyframe Selection)**

- 단조로운 환경에서도 다양한 시점에서 특징을 확보할 수 있도록, 여러 개의 프레임을 고려하여 특징을 추출.

#### **(4) 지형 인식 기반 강화(Landmark-based SLAM)**

- 특징이 적은 환경에서는 별도의 인공 마커(ArUco, AprilTag)를 활용해 로봇이 추적할 수 있도록 함.


### **View 기반 SLAM (View-based SLAM) 개요**

View 기반 SLAM은 기존의 특징 기반(Feature-based) SLAM과 달리, 개별 특징점(feature points) 대신 **이미지 전체의 장면(View)을 직접 활용**하여 로봇의 위치를 추정하는 방식이다. 이는 특히 특징 희소성이 문제되는 환경에서 유리하다.

View 기반 SLAM은 주로 **영상의 전체적인 시각적 정보(appearance)** 또는 **장면(Scene)의 구조적 특성**을 활용하여 위치를 추정하며, 딥러닝 기법과의 결합을 통해 더욱 강력한 성능을 발휘한다.

---

## **1. View 기반 SLAM의 핵심 개념**

View 기반 SLAM은 크게 두 가지 접근 방식으로 나눌 수 있다.

### **(1) 전역 장면 서술(Global Scene Description) 기반 SLAM**

- 개별 특징점이 아니라, **이미지 전체를 하나의 벡터(Feature Descriptor)로 변환**하여 SLAM을 수행하는 방식.
- 대표적인 기법: **Bag of Visual Words (BoVW), GIST Descriptor, CNN Feature Embedding**
- 주로 루프 클로징(Loop Closing)과 재방문 감지에서 강점을 가짐.

### **(2) 뷰 그래프(View Graph) 기반 SLAM**

- View 간의 상대적인 관계를 그래프 구조로 모델링하여 SLAM을 수행하는 방식.
- 특징점이 희소한 환경에서도 **이미지 간의 유사도를 이용하여 위치를 추정**할 수 있음.
- 대표적인 기법: **FAB-MAP, SeqSLAM, Deep Learning-based Place Recognition**

---

## **2. View 기반 SLAM의 주요 기법**

### **(1) BoVW (Bag of Visual Words) 기반 SLAM**

- 기존의 자연어 처리에서 사용하는 **Bag of Words (BoW)** 개념을 영상 데이터에 적용.
- 이미지의 주요 패치를 클러스터링하여 ‘단어(words)’처럼 저장하고, 이를 기반으로 이미지 비교 수행.
- 특징 기반 SLAM보다 **특징점이 부족한 환경에서도 강건한 성능**을 보일 수 있음.

### **(2) GIST Descriptor 기반 SLAM**

- 이미지의 전체적인 패턴을 추출하여 **전역적인 환경 정보**를 기반으로 SLAM 수행.
- 특징점이 부족한 단조로운 환경에서도 강건한 매칭 가능.
- 다만, 기존의 SIFT, ORB 기반의 방법보다 정확도가 낮은 경우가 있음.

### **(3) SeqSLAM (Sequence-based SLAM)**

- 개별 프레임이 아니라 **연속적인 이미지 시퀀스(Sequence)** 를 비교하여 위치를 추정.
- GPS가 없는 환경에서 **비주얼 오도메트리(Visual Odometry)** 를 보완하는 데 사용 가능.
- 해양 및 공중 드론 환경에서 자주 사용됨.

### **(4) FAB-MAP (Fast Appearance-Based Mapping)**

- 특징 기반이 아니라, **장면(Scene) 자체의 외형적 특징**을 바탕으로 장소 인식을 수행.
- 카메라 기반 루프 클로징 및 위치 재인식(Place Recognition)에서 우수한 성능을 보임.

### **(5) 딥러닝 기반 View SLAM**

- CNN을 활용하여 **이미지의 전역적인 특징(Deep Feature)** 을 추출하고, 이를 기반으로 위치 추정 수행.
- 대표적인 모델:
    - **NetVLAD**: View 기반 장소 인식에 특화된 신경망 구조.
    - **DeepVO**: 딥러닝 기반 비주얼 오도메트리.

---

## **3. View 기반 SLAM의 장점과 한계**

### ✅ **장점**

1. **특징 희소성 문제 해결**
    
    - 환경이 단조로워 특징을 추출하기 어려운 경우에도 SLAM 수행 가능.
    - 특히 실내 환경, 수중 환경, 우주 탐사 등에서 효과적.
2. **루프 클로징(Loop Closing) 강점**
    
    - 과거 방문한 지역을 재인식하는 데 뛰어난 성능.
    - Global Scene Descriptor 방식을 활용하면 더 안정적인 루프 클로징 가능.
3. **딥러닝과의 결합이 용이**
    
    - CNN, Transformer 등의 딥러닝 기법과 결합하여 더욱 강력한 성능을 발휘할 수 있음.

### ❌ **한계**

1. **위치 추정의 정밀도 부족**
    
    - 특징 기반 SLAM은 개별 특징점을 정밀하게 매칭하는 반면, View 기반 SLAM은 전역적인 장면 정보를 사용하기 때문에 정밀도가 낮을 수 있음.
2. **컴퓨팅 비용 증가**
    
    - 딥러닝을 활용한 View 기반 기법은 연산량이 크고, 실시간 처리가 어려울 수 있음.
3. **센서 융합 필요**
    
    - 단순히 영상 기반으로만 진행하면 Drift가 발생할 수 있어, IMU, LiDAR와 결합하는 것이 바람직함.
### **지연 상태 필터(Delayed State Filter)란?**

**지연 상태 필터(Delayed State Filter, DSF)**는 SLAM(동시적 위치추정 및 지도 작성)이나 상태 추정에서 사용되는 필터링 기법으로, **과거의 상태(state)를 유지하면서 새로운 정보가 도착했을 때 이를 반영하여 정확도를 개선하는 방법**이다.

이 방법은 특히 **센서 데이터가 지연되거나, 루프 클로저(Loop Closure)가 발생하거나, 고지연(high-latency) 업데이트가 필요한 경우**에 유용하다.

지연 상태 필터는 **특정한 SLAM 구조(View-based SLAM에 한정된 개념이 아님)**라기보다는, **특징 기반 SLAM, View 기반 SLAM, 센서 융합 SLAM 등 다양한 SLAM 방식에 적용될 수 있는 기법**이다.

---

## **1. 지연 상태 필터(DSF)가 필요한 이유**

### **(1) 센서 데이터의 지연 문제 해결**

- 카메라, LiDAR, IMU 등의 센서는 데이터 처리 속도가 다름.
- 카메라는 특징점(feature) 추출이 느려지고, LiDAR는 스캔 속도가 낮아 데이터 업데이트가 지연될 수 있음.
- DSF는 이런 지연된 데이터를 **과거 상태에 반영하여 정확도를 높임**.

### **(2) 루프 클로저(Loop Closure) 개선**

- SLAM에서 로봇이 이전에 방문한 장소를 인식하면, **과거의 위치 추정 값을 보정할 필요**가 있음.
- 일반적인 필터(EKF, UKF 등)는 과거 상태를 유지하지 않기 때문에 루프 클로저 정보를 반영하기 어려움.
- DSF는 과거 상태를 저장하고 있으므로, 루프 클로저가 발생했을 때 **기존 경로를 재조정**하여 오차를 줄일 수 있음.

### **(3) 누적 오차(Drift) 감소**

- **IMU 기반 오도메트리(Visual-Inertial Odometry, VIO)**에서는 작은 오차가 누적되면 위치 추정이 크게 틀어질 수 있음.
- DSF는 새로운 정보가 들어오면 **과거의 상태를 보정하면서, 이후 모든 상태를 업데이트**하기 때문에 누적 오차를 줄이는 데 도움을 줌.

---

## **2. 지연 상태 필터의 동작 원리**

### **(1) 상태(state) 표현**

SLAM에서 시간 ttt에서의 상태는 다음과 같이 표현할 수 있음:

$$xt={pt,vt,θt,bt}\mathbf{x}_t = \{ \mathbf{p}_t, \mathbf{v}_t, \mathbf{\theta}_t, \mathbf{b}_t \}xt​={pt​,vt​,θt​,bt​}$$

- $$pt\mathbf{p}_tpt​ = 위치(Position)$$
- vt\mathbf{v}_tvt​ = 속도(Velocity)
- θt\mathbf{\theta}_tθt​ = 자세(Orientation, 쿼터니언 등)
- bt\mathbf{b}_tbt​ = 센서 바이어스(Bias)

보통의 필터는 과거의 상태를 유지하지 않지만, **DSF는 과거의 여러 상태를 유지하면서 새로운 정보가 들어오면 이를 과거에 반영**함.

### **(2) 업데이트 과정**

1. **예측 단계 (Prediction)**
    
    - 로봇이 움직이는 동안 IMU(관성센서) 데이터를 활용하여 상태를 예측.
    - 일반적인 이동 모델: xt+1=f(xt,ut)+wt\mathbf{x}_{t+1} = f(\mathbf{x}_t, \mathbf{u}_t) + \mathbf{w}_txt+1​=f(xt​,ut​)+wt​ 여기서 ut\mathbf{u}_tut​는 입력(control input), wt\mathbf{w}_twt​는 노이즈.
2. **지연된 센서 측정값 반영 (Delayed Measurement Update)**
    
    - 센서 데이터가 늦게 도착했을 경우, 기존 필터들은 이를 무시하거나 현재 상태에 반영함.
    - **DSF는 과거의 상태 xt−d\mathbf{x}_{t-d}xt−d​ 에 지연된 데이터 zt−d\mathbf{z}_{t-d}zt−d​ 를 반영**하여 업데이트.
    - 예: 루프 클로저가 발생하면, DSF는 **과거 위치를 수정하고, 이후 모든 상태를 업데이트**함.
3. **보정 정보 전파 (Propagation of Corrections)**
    
    - 과거 상태가 수정되면, 이후의 모든 상태도 함께 보정됨.
    - 이로 인해 **누적 오차를 효과적으로 줄일 수 있음**.

---

## **3. 지연 상태 필터와 SLAM 적용 사례**

### **(1) 특징 기반 SLAM에서의 적용**

- **비주얼-관성 오도메트리(VIO)**에서는 IMU는 빠른 업데이트를 제공하지만, **카메라는 특징 추출이 느려짐**.
- DSF를 사용하면 **카메라 특징점이 나중에 도착하더라도, 과거의 IMU 기반 상태를 수정할 수 있음**.

### **(2) View 기반 SLAM에서의 적용**

- View 기반 SLAM에서는 **이미지 기반 루프 클로저 검출이 지연됨**.
- DSF는 루프 클로저 정보가 나중에 도착해도 **과거 상태를 업데이트하여 경로 왜곡을 줄일 수 있음**.

### **(3) 다중 센서 융합 SLAM에서의 적용**

- LiDAR, 카메라, IMU를 함께 사용하는 경우, **LiDAR는 업데이트가 느려지고, IMU는 빠르게 업데이트됨**.
- DSF를 적용하면, **지연된 LiDAR 데이터를 나중에 적용해도 과거 경로를 수정할 수 있음**.

---

## **4. 다른 필터들과 비교**

|필터 방식|장점|단점|
|---|---|---|
|**EKF/UKF (확장 칼만 필터/무향 칼만 필터)**|실시간 가능, 계산량이 적음|과거 상태를 업데이트할 수 없음|
|**슬라이딩 윈도우 최적화 (Sliding Window Optimization)**|최근 상태의 정확도가 높음|오래된 상태는 업데이트 불가|
|**지연 상태 필터 (DSF)**|지연된 정보 반영 가능, 루프 클로저 적용 가능|계산량 증가, 메모리 사용량 증가|

### **EKF에서 상태 벡터 크기 증가가 제곱 복잡도로 이어지는 이유**

---

EKF(확장 칼만 필터, Extended Kalman Filter)는 **SLAM(동시적 위치추정 및 지도 작성)** 에서 중요한 역할을 하지만, **상태 벡터(State Vector)의 크기가 커질수록 계산량이 기하급수적으로 증가**하는 문제가 있습니다.  
이 문제는 **공동 상관관계(Joint Correlation)를 유지하는 EKF의 구조** 때문입니다. 이를 하나씩 설명해보겠습니다.

---

## **1. EKF에서 상태 벡터와 공분산 행렬**

EKF는 상태 벡터(x\mathbf{x}x)와 공분산 행렬(P\mathbf{P}P)을 사용하여 로봇의 위치와 환경의 지도(랜드마크)를 동시에 추정합니다.

### **(1) 상태 벡터 (State Vector)**

상태 벡터 x\mathbf{x}x는 로봇의 상태(위치, 속도 등)와 환경의 랜드마크들을 포함합니다.

x=[xrobotyrobotθrobotx1y1x2y2⋮xNyN]\mathbf{x} = \begin{bmatrix} x_{\text{robot}} \\ y_{\text{robot}} \\ \theta_{\text{robot}} \\ x_1 \\ y_1 \\ x_2 \\ y_2 \\ \vdots \\ x_N \\ y_N \end{bmatrix}x=​xrobot​yrobot​θrobot​x1​y1​x2​y2​⋮xN​yN​​​

여기서:

- (xrobot,yrobot,θrobot)(x_{\text{robot}}, y_{\text{robot}}, \theta_{\text{robot}})(xrobot​,yrobot​,θrobot​) 는 로봇의 위치 및 방향
- (xi,yi)(x_i, y_i)(xi​,yi​) 는 랜드마크의 좌표 (i=1,2,…,Ni = 1, 2, \dots, Ni=1,2,…,N, 랜드마크 개수)

즉, 로봇이 새로운 랜드마크를 인식할 때마다 상태 벡터 x\mathbf{x}x의 크기가 증가합니다.

---

### **(2) 공분산 행렬 (Covariance Matrix)**

EKF는 상태 벡터뿐만 아니라, 상태 간의 **불확실성을 나타내는 공분산 행렬(P\mathbf{P}P)**도 유지해야 합니다. 공분산 행렬 P\mathbf{P}P는 **모든 상태 변수 간의 상관관계**를 포함하는 (M×M)(M \times M)(M×M) 크기의 대칭 행렬입니다.  
(여기서 MMM은 상태 벡터의 크기, 즉 M=3+2NM = 3 + 2NM=3+2N, 로봇의 상태 3개 + 랜드마크 2개 × NNN개)

$$P=[PxxPxyPxθPx1Py1…PxNPyNPyxPyyPyθPy1Px1…PxNPyNPθxPθyPθθPθ1Pθy1…PθxNPθyNP1xP1yP1θP11P12…P1NP1N⋮⋮⋮⋮⋮⋱⋮⋮PNxPNyPNθPN1PN2…PNNPNN]\mathbf{P} = \begin{bmatrix} P_{xx} & P_{xy} & P_{x\theta} & P_{x1} & P_{y1} & \dots & P_{xN} & P_{yN} \\ P_{yx} & P_{yy} & P_{y\theta} & P_{y1} & P_{x1} & \dots & P_{xN} & P_{yN} \\ P_{\theta x} & P_{\theta y} & P_{\theta \theta} & P_{\theta 1} & P_{\theta y1} & \dots & P_{\theta xN} & P_{\theta yN} \\ P_{1x} & P_{1y} & P_{1\theta} & P_{11} & P_{12} & \dots & P_{1N} & P_{1N} \\ \vdots & \vdots & \vdots & \vdots & \vdots & \ddots & \vdots & \vdots \\ P_{Nx} & P_{Ny} & P_{N\theta} & P_{N1} & P_{N2} & \dots & P_{NN} & P_{NN} \end{bmatrix}P=​Pxx​Pyx​Pθx​P1x​⋮PNx​​Pxy​Pyy​Pθy​P1y​⋮PNy​​Pxθ​Pyθ​Pθθ​P1θ​⋮PNθ​​Px1​Py1​Pθ1​P11​⋮PN1​​Py1​Px1​Pθy1​P12​⋮PN2​​…………⋱…​PxN​PxN​PθxN​P1N​⋮PNN​​PyN​PyN​PθyN​P1N​⋮PNN$$​​​

이 공분산 행렬은 **각 상태 변수 간의 공통 상관관계**를 유지해야 하기 때문에 크기가 커질수록 계산량이 급격히 증가합니다.

---

## **2. EKF의 계산량 증가 이유 (제곱 복잡도, O(M2)O(M^2)O(M2))**

EKF의 연산 중 가장 계산량이 높은 부분은 **칼만 이득(Kalman Gain) 계산 및 공분산 행렬 갱신**입니다.

### **(1) 칼만 이득 계산**

EKF의 주요 업데이트 과정 중 하나는 **칼만 이득(Kalman Gain) K\mathbf{K}K 계산**입니다. 이 과정에서 공분산 행렬 P\mathbf{P}P의 역행렬을 계산해야 합니다.

K=PHT(HPHT+R)−1\mathbf{K} = \mathbf{P} \mathbf{H}^T (\mathbf{H} \mathbf{P} \mathbf{H}^T + \mathbf{R})^{-1}K=PHT(HPHT+R)−1

- 공분산 행렬 P\mathbf{P}P의 크기는 M×MM \times MM×M이며, 이 행렬을 역행렬 계산하는 연산은 **O(M3)O(M^3)O(M3) (행렬 역행렬 연산의 일반적인 시간 복잡도)**
- 그러나 SLAM에서는 일부 최적화 기법을 사용하여 O(M2)O(M^2)O(M2)로 줄일 수 있음.

### **(2) 공분산 행렬 갱신**

EKF는 매 업데이트마다 **공분산 행렬 P\mathbf{P}P 전체를 갱신**해야 합니다.

P=(I−KH)P\mathbf{P} = (\mathbf{I} - \mathbf{K} \mathbf{H}) \mathbf{P}P=(I−KH)P

- 행렬 P\mathbf{P}P의 크기가 M×MM \times MM×M이므로, 업데이트 연산에서 **최소 O(M2)O(M^2)O(M2) 의 계산량이 필요**합니다.

### **(3) 전체 계산량**

EKF의 전체적인 시간 복잡도는 **칼만 이득 계산(O(M2)O(M^2)O(M2)) + 공분산 행렬 갱신(O(M2)O(M^2)O(M2))** 으로 인해 **총 O(M2)O(M^2)O(M2)의 복잡도를 가짐**.

즉, **랜드마크 개수 NNN이 증가할수록 O((3+2N)2)O((3 + 2N)^2)O((3+2N)2)의 계산량이 필요**하며, 이는 로봇이 이동하면서 더 많은 랜드마크를 인식할수록 계산 속도가 급격히 느려지는 원인이 됩니다.

---

## **3. 왜 EKF는 작은 맵에서만 효과적인가?**

- 상태 벡터 크기 MMM이 증가하면 **공분산 행렬 P\mathbf{P}P의 크기**도 함께 증가하며, 계산량이 **제곱 비율(O(M2)O(M^2)O(M2))로 증가**함.
- 즉, **랜드마크 수가 많아질수록 계산량이 기하급수적으로 증가**하여, EKF는 **작은 환경(소규모 맵)에서는 적절하지만, 대규모 환경에서는 비효율적**임.

---

## **4. 해결 방법: 희소성 활용**

### **(1) 정보 필터(Information Filter)**

- EKF와 다르게, 정보 필터는 **공분산 행렬이 아니라 "정보 행렬(Information Matrix)"을 이용**하여 계산량을 줄임.
- 일부 요소만 업데이트하여 계산량을 O(N)O(N)O(N) 수준으로 줄일 수 있음.

### **(2) 그래프 기반 SLAM (Graph-Based SLAM)**

- 전체 상태 벡터를 유지하는 대신, **로봇과 랜드마크 간의 관계를 그래프 형태로 모델링**하여 계산량을 줄이는 방법.
- 대표적인 예: **GTSAM, iSAM, ORB-SLAM 등**

### **(3) 지연 상태 필터(ESDFs)**

- 과거 상태를 전부 유지하는 대신, **중요한 정보만 선택적으로 저장하여 계산량을 줄이는 기법**.