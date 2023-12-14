# BigDataSecurity Team Project 

## Main paper : [Malware Detection through memory forensics.pdf](https://github.com/ckj18/BigDataSecurity/files/11396072/Malware.Detection.through.memory.forensics.pdf)


## Subject

### 해당 프로젝트는 메모리 덤프 파일을 활용하여 비정상적인 프로세스를 탐지하기 위한 모델을 찾는다. 악성코드를 탐지하기 위한 방법으로는 정적, 동적 및 메모리 기반의 방식이 있다. 

## Motivation

### 메모리 덤프를 이미지로 변환하고 GIST와 HOG로 전처리 한 후에 머신러닝 모델을 학습해 높은 성능을 보이는 것을 보고 SIFT와 같은 근래에 주로 사용되는 GIST와 HOG를 제외한 디스크립터를 도입했을 때의 성능 변화가 기대되었으며, 딥러닝 모델을 활용한 메모리 기반 분석 방식에 대한 정보가 없어 딥러닝 모델을 활용했을 때의 기존과의 성능 차이도 기대되었다.

## Dataset

- “Dumpware 10” Dataset은 10개의 악성코드 클래스와 1개의 양성 샘플 클래스로 구성된 총 4294개의 Portable Executables를 포함하는 데이터셋이다. 이 데이터셋은 터키에 위치한 정보 보안 회사와의 협력을 통해 수집된 실제 악성코드 Pes와 Windows 운영 체제에서 대부분 가져온 적절한 수의 양성 실행 파일(PEs)을 결합하여 구축되었다. 이 데이터셋은 학습 세트와 검증 세트로 분할되어 있으며, 80%/ 20%의 분할 비율을 사용하여 구성되었다.
- “Dumpware 10” Dataset를 통해 메모리 덤프를 생성하기 위해Microsoft에서 개발한 명령줄 응용 프로그램인 “Procdump”를 사용해 Dataset으로부터 메모리 덤프 파일을 생성한다

## Preprocessing

### Laplacian Filtering
- Laplacian Filter는 이미지 분석 분야에서 edge를 검출하기 위해 사용한다.  주로 Edge 검출을 위해서는 연속된 픽셀 값에 미분을 하여 픽셀 값이 급격하게 변하는 지점을 찾아 경계(엣지)를 검출한한다.
-  Laplacian Filter는 2차 미분을 활용해 에지를 검출한다. 이를 통해 다른 edge 검출 필터보다 저금 더 정확한 edge를 얻을 수 있다. Edge를 통해 특징을 추출하는 이미지 디스크립터의 정확도를 높이기 위해 해당 필터를 통해 Edge를 검출한 후 원본 이미지와 합쳐 Edge를 강화했다.

### Bilateral Filtering
-  Bilateral Filter를 이미지 분석에서 노이즈를 제거하기 위해 사용한다. 기존 Gaussian Filter는 Blur 처리를 통해 노이즈를 제거하는데 이는 노이즈를 제거하는데 효과적이지만, 경계선이 뭉개지는 문제점이 있다. Bilateral Filter는 가우시안 필터와 경계 필터 2개를 사용해 잡음을 제거하는데 효과가 있지만, 경계(edge)도 흐릿하게 하는 블러링 필터의 문제를 개선한 방식으로, 노이즈는 없고 경계가 비교적 뚜렷한 이미지 얻을 수 있다. 해당 필터를 사용해 이미지의 노이즈를 제거했다.

### GIST
-  GIST Descriptor 는 2001 년에 Olivia와 Torrabla 과 고안한 Descriptor 이이다. 이는 하나의 필터링 결과만을 가지고 비교하기에는 이르니 여러 범위의 여러 필터들을 보고 비교를 하겠다는 간단한 아이디어에서 시작한 방식이다.
-  벡터의 dimensionality(차원)는 총 16N이다. 이 벡터를 비교함으로써 유사도를 측정하는 것이 바로 GIST Descriptor 이다.
-  사용하는 Gabor Filter는 위와 같은 방정식으로 파동을 표현하는 함수이다. 여기서 σ 값이 커질 수록 피크 값이 낮고 옆으로 퍼지는 형태가 된된다. 또한 w 값은 클수록 주기가 짧아지고 진동수가 작아져 촘촘한 형태가 된다. 현재는 이 함수의 미지수가 x 하나이기 때문에 1차원 방정식이지만 y 까지 고려하여 2차원 형태를 취한 것이 바로 Gabor Filter 이다.
  
### HOG
-  HOG Descriptor 는 2005년 Dalal와 Triggs가 고안한 방식이다. HOG 는 gradient 의 histogram 을 그린다는 간단한 아이디어에서 개발되었다. GIST 와 마찬가지로 cell 을 나누고, gradient 를 구해서 direction(방향)별로 magnitude(크기)를 histogram 으로 구한다.
-  파란색을 보면 바향 80에 해당하는 픽셀에 대해서 크기가 2이기 때문에 histogram 의 80에 2를 더한다. 빨강을 보면 방향 10에 해당하는 픽셀의 크기가 4이이다. 10은 histogram 상에서 0과 20의 중간이기 때문에 크기 4를 반으로 나누어 각각에 2씩을 더한다.
-  그리고 각각에 cell 에 대해 구한 histogram 값을 하나의 벡터로 합쳐 이를 비교함으로써 유사도를 구하는 것이 바로 HOG Descriptor 이다.

### SIFT
-  앞서 설명한 GIST와 HOG는 특징점을 기술하는 것에 그쳤다. 즉, 특징점의 유사도를 파악하는 것이었다. 그런데 특징점을 기술하기 위해서는 일단 이미지에 특징점을 찾는 detect 과정이 선행되어야 한다. SIFT는 feature detector 와 featue descriptor 를 모두 수행해주는 알고리즘이다. 이 SIFT 는 컴퓨터 비전에 본격적으로 Neural Network 가 도입되기 전에는 가장 널리 사용되었던 디스크립터로, Neural Network를 적용하기 힘든 일부 어플리케이션에서는 여전히 많이 사용되고 있다.
-  SIFT 의 Detection 에서는 다양한 크기의 주목해서 봐야할 영역을 찾고 이를 localization 와 normalzation 을 거쳐 gradient 를 써서 orientation 을 구하고 그것들을 최종적으로 고차원 vector 형태로 나타내게 된다.
-  SIFT 의 실질적 구현은 여러 σ, 즉 여러 Scale 을 가지는 Gaussian 값들에 대해서 이웃한 것들을 빼면 그 결과는 여러 σ, 즉 여러 Scale 을 가지는 DOG 를 Laplacian 의 근사치로써 구할 수 있는 것이다.
-   Normalization을 거친 기준이 되는 크기는 16 X 16 픽셀이다. 모든 픽셀에는 각각 image gradient 값이 계산되어 있다. 이 gradient 값에 대해서 패치 너비의 반이 σ 값을 가지게끔 패치의 중앙이 가장 크게 반영되는 Gaussian Weighting 을 수행한다. 그리고 각각을 4 X 4 픽셀 크기의 16개의 셀로 구분한한다. 그리고 이들을 8개의 방향에 대해서 인코딩을 해서 히스토그램처럼 나타내면 16개의 셀에 대해 8개의 방향이므로 총 128개의 값이 도출된다. 이를 나열한 것이 바로 SIFT Descriptor 가 비교하는 최종 값이다.

## EDA
### Statistics (Label Distribution)
-  Pie graph를 통해 Train과 Test에 정상 데이터(Other)와 10개의 malware 클래스로 분류되어있다는 것을 알 수 있다. 10개의 malware class는는 Autorun, Amonetize, Allaple, Adposhel, Vilsel, VBA, MultiPlug, installCore, Dinwod, BrowseFox로 이루어져 있다. 
 Train에 사용한 Data의 경우 총 10852개의 Data를 사용했다. 정상 데이터는 Train Data의 21.1%이며, 각 malware class는 Train Data의 7.9%로 분포되어 있다.
 Test에 사용한 Data의 경우 총 2714개의 Data를 사용했다. 이를  1357개로 반으로 나누어 각각 validation data와 test data로 사용했다. validation data와 test data는 위의 Pie graph와 같이 Test 데이터의 절반에 가까운 50.5%가 정상데이터이며, 나머지는 10개의 malware 데이터가 최소 4.3%에서 최대 5.3%로 분포되어 있다.

### T-SNE
-  전처리 과정을 거치기 전 Train data와 Test data에 대한 T-SNE이다. 위의 T-SNE를 통해 정상 데이터와 malware 데이터가 군집화를 이루지 않은채 구분없이 산개되어 있는 것을 볼 수 있다.


## Rules

### 1. 모델의 최종학습은 팀장의 개발 환경에서 실행 
### 2. 팀원은 공유 드라이브에 있는 데이터셋을 Colab 개발환경에서 활용하여 각자의 역할 수행
### 3. 역할 수행 후 변경된 사항은 Github을 통해 Push
### 4. 요청사항이나 질문은 Commit을 이용
### 5. 팀원별 역할은 각자 수정

## 팀원별 역할

### 공통 : 전처리, EDA, 하이퍼파라미터 튜닝, 결과분석, 보고서 작성
### 최경주 : 딥러닝 모델 구성, 최종발표
### 최준태 : 전처리, EDA, 중간발표
### 박현재 : 전처리, EDA, 최종발표
### 문한새 :
