# Fake Audio Detection

이 프로젝트는 딥러닝을 활용하여 **Fake Audio Detection (가짜 음성 탐지)** 문제를 해결하는 것을 목표로 합니다. 전처리 과정에서 다양한 음향 데이터를 **Melspectrogram**, **Spectrogram**, **MFCC**, **LFCC**와 같은 형식으로 변환하고, GRU, LSTM, Transformer 등 여러 모델을 비교하였습니다. 최종적으로 **앙상블 모델**을 활용하여 성능을 극대화하였습니다.

---

## 실행 결과
### 혼동 행렬 (Confusion Matrix)
모델별 혼동 행렬은 아래와 같습니다. 이미지 파일 경로와 함께 설명을 포함합니다:

1. **GRU**
   ![GRU 혼동 행렬](https://github.com/user-attachments/assets/2f116bbe-2604-4993-a6c3-bcdde21ab5ac)
   - GRU 기반 모델은 훈련이 빠르고 계산 효율성이 높아 경량 모델 구현에 적합합니다.

3. **LSTM**
   ![LSTM 혼동 행렬](https://github.com/user-attachments/assets/7a357c60-aa0c-4480-b1f5-42a92dcd6c5f)
   - LSTM은 시간적 종속성을 잘 학습하며, 적은 데이터에서도 안정적인 성능을 보입니다.

5. **Transformer**
   ![Transformer 혼동 행렬](https://github.com/user-attachments/assets/bd4a9d5a-690d-41ec-8afd-6bb3cefc1bfb)
   - Transformer는 복잡한 데이터 구조를 학습하기에 적합하며, 장기적 의존성을 효과적으로 학습합니다.

6. **Voting Ensemble**
   ![Voting 앙상블 혼동 행렬](https://github.com/user-attachments/assets/72845332-0db7-492c-8914-1fa9032f9d44)
   - 각 모델의 예측을 단순히 결합하여 최종 출력을 생성한 결과입니다.

8. **Weighted Voting Ensemble**
   ![Weighted Voting 앙상블 혼동 행렬](https://github.com/user-attachments/assets/d7995b0b-d40d-4486-a098-3e2603e55bc4)
   - 각 모델의 성능에 따라 가중치를 부여한 앙상블 기법입니다. 가중치: LSTM (0.5), GRU (0.3), Transformer (0.2).

---

## 목차
1. [프로젝트 개요](#프로젝트-개요)
2. [전처리 과정](#전처리-과정)
3. [모델 구조](#모델-구조)
4. [결과 및 분석](#결과-및-분석)
5. [팀 구성 및 역할](#팀-구성-및-역할)

---

## 프로젝트 개요
### 목표
- 다양한 환경에서 가짜 음성(Fake Audio)을 탐지하는 AI 모델 구축
- 전처리된 오디오 데이터를 기반으로 GRU, LSTM, Transformer 모델 성능 비교 및 최적화
- 성능 향상을 위한 앙상블 기법 적용

### 데이터셋
- **출처**: Kaggle - The Fake-or-Real (FoR) Dataset
- **구성**:
  - **for-2sec**: 2초로 잘린 발화 데이터
  - **for-norm**: 성별 및 클래스 균형이 맞춰진 표준화된 데이터셋
  - **for-original**: 원본 데이터
  - **for-rerec**: 재녹음된 데이터

---

## 전처리 과정
### 음성 데이터를 전처리하여 아래와 같은 형식으로 변환:
1. **Melspectrogram** 및 **Spectrogram**: 이미지 데이터로 변환
2. **MFCC** 및 **LFCC**: 시간 축에 따라 특징 벡터로 변환
3. 데이터 증강:
   - 발화가 짧은 경우 반복
   - 발화가 긴 경우 자르기

### 선택된 전처리 방식
- **MFCC**와 **LFCC**는 학습 속도가 빠르고 적은 데이터에서도 높은 성능을 보여 최종 채택
- **Melspectrogram** 및 **Spectrogram**은 자원의 한계로 제외

---

## 모델 구조
### GRU
- **특징**: 단순한 구조와 빠른 학습 속도
- **구조**: Bidirectional GRU + 정규화 및 드롭아웃

### LSTM
- **특징**: 시간적 종속성을 잘 학습
- **구조**: Bidirectional LSTM + 정규화 및 드롭아웃

### Transformer
- **특징**: 장기적 의존성을 효과적으로 학습
- **구조**: Conv1D 초기 특징 추출 + Multi-Head Attention + Feedforward 반복

### 앙상블 기법
1. **Voting Ensemble**
   - 각 모델의 예측을 단순 다수결로 결합
2. **Weighted Voting**
   - LSTM (0.5), GRU (0.3), Transformer (0.2)로 가중치를 설정

---

## 결과 및 분석
- **평가 지표**: 혼동 행렬, F1 Score, Binary Crossentropy Loss
- **최종 성능**:
  - **LSTM 단일 모델**: 가장 높은 성능
  - **Weighted Voting 앙상블**: 추가적인 성능 향상

---

## 팀 구성 및 역할
- **오승진 (20223179)**:
  - LSTM 모델 학습 및 구현
  - MFCC 및 LFCC 전처리
- **이호성 (20223182)**:
  - GRU 모델 학습 및 구현
  - MFCC 및 LFCC 전처리
- **이성준 (20223181)**:
  - Melspectrogram, Spectrogram 전처리 시도
  - Transformer 모델 및 앙상블 구현
  - 최종 결과 발표 및 비교 분석

---

## 참고 문헌
- [Kaggle - The Fake-or-Real (FoR) Dataset](https://www.kaggle.com)
- 김, 소운, & 이, 성택. (연도). 딥보이스를 악용한 보이스 피싱 피해방지 서비스 개발.
- 박, 대서, 방, 준일, 김, 화종, & 고, 영준. (2020). CNN을 이용한 음성 데이터 성별 및 연령 분류 기술 연구.

---

이 README는 OpenAI의 ChatGPT에 의해 작성되었습니다.
