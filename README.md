# 문서 이미지 분류 시스템

## 개요

이 프로젝트는 심층 학습 기반의 문서 이미지 분류 시스템을 구현합니다. 고급 이미지 전처리, OCR 기반 텍스트 추출 및 하이브리드 모델 아키텍처를 결합하여 문서 이미지 분류 정확도를 높이는 것을 목표로 합니다.

## 특징

1. 이미지 및 텍스트 기반 하이브리드 모델
2. Augraphy를 활용한 문서 이미지 증강
3. Pytesseract를 이용한 OCR 텍스트 추출
4. 오버샘플링을 통한 클래스 불균형 처리
5. 사용자 정의 손실 함수: Focal Loss + Cross Entropy
6. 동적 학습률 조정
7. 조기 종료(Early Stopping) 메커니즘

## 데이터셋 준비

데이터셋은 다음과 같은 구조를 따라야 합니다:

```
data/
├── images/
│   ├── image1.jpg
│   ├── image2.jpg
│   └── ...
└── labels.csv
```

`labels.csv` 파일의 필수 컬럼:

- `ID`: 이미지 파일 이름
- `target`: 분류 레이블

## 전처리 파이프라인

### 1. 이미지 처리

- LANCZOS 방식 이미지 업스케일링
- 밝기 및 대비 향상
- 선명도 조정
- Augraphy 기반 증강 기법 적용:
  - 더러운 롤러 효과(Dirty rollers)
  - 노이즈 텍스처 적용
  - 접힘 효과(Folding effects)
  - 잉크 번짐 및 얼룩 효과
  - 가우시안 블러
  - 지터 효과(Jitter effects)
  - 조명 그라디언트
  - 워터마크 오버레이

### 2. OCR 처리

- Pytesseract를 이용한 한국어 및 영어 텍스트 추출
- 텍스트 전처리:
  - 특수 문자 제거
  - TF-IDF 벡터화
  - 차원 축소 (TruncatedSVD, 50개 특징 추출)

## 접근 방향

### 1. 모델 아키텍처

1. 이미지 및 텍스트 특징을 결합한 하이브리드 접근 방식
2. 특정 클래스별 텍스트 벡터 가중치 조정
3. 사용자 정의 손실 함수 적용:
   - Focal Loss를 통한 클래스 불균형 처리
   - Cross Entropy Loss
   - 가변적 가중치 적용

### 2. 성능 최적화

1. 클래스 균형을 맞추기 위한 오버샘플링 (최대 4배)
2. ReduceLROnPlateau를 활용한 동적 학습률 조정
3. 조기 종료(Early Stopping)로 과적합 방지
4. 주요 문서 클래스(3, 4, 7, 14)에 대해 텍스트 가중치 1.5배 적용

## 개발 기간

- 전체 개발 기간 : 2024-10-29 ~ 2024-11-08

