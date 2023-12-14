# BigDataSecurity Team Project 

## Main paper : [Malware Detection through memory forensics.pdf](https://github.com/ckj18/BigDataSecurity/files/11396072/Malware.Detection.through.memory.forensics.pdf)


## Subject

### 해당 프로젝트는 메모리 덤프 파일을 활용하여 비정상적인 프로세스를 탐지하기 위한 모델을 찾는다. 악성코드를 탐지하기 위한 방법으로는 정적, 동적 및 메모리 기반의 방식이 있다. 

## Motivation

### 메모리 덤프를 이미지로 변환하고 GIST와 HOG로 전처리 한 후에 머신러닝 모델을 학습해 높은 성능을 보이는 것을 보고 SIFT와 같은 근래에 주로 사용되는 GIST와 HOG를 제외한 디스크립터를 도입했을 때의 성능 변화가 기대되었으며, 딥러닝 모델을 활용한 메모리 기반 분석 방식에 대한 정보가 없어 딥러닝 모델을 활용했을 때의 기존과의 성능 차이도 기대되었다.

## Dataset

- “Dumpware 10” Dataset은 10개의 악성코드 클래스와 1개의 양성 샘플 클래스로 구성된 총 4294개의 Portable Executables를 포함하는 데이터셋이다. 이 데이터셋은 터키에 위치한 정보 보안 회사와의 협력을 통해 수집된 실제 악성코드 Pes와 Windows 운영 체제에서 대부분 가져온 적절한 수의 양성 실행 파일(PEs)을 결합하여 구축되었다. 이 데이터셋은 학습 세트와 검증 세트로 분할되어 있으며, 80%/ 20%의 분할 비율을 사용하여 구성되었다.
- “Dumpware 10” Dataset를 통해 메모리 덤프를 생성하기 위해Microsoft에서 개발한 명령줄 응용 프로그램인 “Procdump”를 사용해 Dataset으로부터 메모리 덤프 파일을 생성한다

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
