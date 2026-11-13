# 인공지능과 정보보호 실습 상세 기록

## 학습 범위
본 문서는 전공 실습에서 다룬 머신러닝 분류 및 비지도학습의 핵심 원리와 검증 방법을 정리한다. 각 실습의 실제 수치와 그래프는 해당 노트북의 출력 셀을 기준으로 확인한다. 아래 예시 코드는 개념 설명용이며 제출 노트북의 실행 결과를 대신하지 않는다.

## 1. 악성코드 분류: K-최근접 이웃(KNN)

### 목적
정상/악성 샘플을 특징 벡터로 표현하고, 가까운 학습 샘플들의 레이블을 바탕으로 미지의 샘플을 분류한다.

### 진행 과정
1. 데이터 행 수, 열 수, 레이블의 클래스별 개수를 확인한다.
2. 결측치·중복·레이블 누수 가능성을 점검한다.
3. 학습/테스트 데이터를 분리한다. 같은 파일에서 파생된 샘플이 양쪽에 중복되지 않도록 주의한다.
4. 거리 기반 모델의 특성을 고려해 학습 데이터로만 스케일러를 학습한다.
5. 이웃 수(k)에 따른 성능 변화를 비교한다.
6. 혼동행렬에서 악성 샘플의 미탐(FN)과 정상 샘플의 오탐(FP)을 구분한다.

### 핵심 코드 패턴
```python
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import classification_report, confusion_matrix

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
model = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5))
model.fit(X_train, y_train)
pred = model.predict(X_test)
print(confusion_matrix(y_test, pred))
print(classification_report(y_test, pred, zero_division=0))
```

### 해석
KNN은 직관적이지만 특징의 단위에 민감하고 고차원 데이터에서 거리의 구별력이 약해질 수 있다. 보안 탐지에서는 전체 정확도만 높아도 악성 샘플을 놓칠 수 있으므로 악성 클래스의 재현율과 오탐률을 별도로 해석한다.

## 2. 피싱 탐지: 로지스틱 회귀

### 목적
URL 또는 사이트의 수치화된 특성으로 피싱 여부의 확률을 추정하고 임계값에 따라 분류한다.

### 진행 과정
1. 특징의 의미와 타깃 레이블을 확인한다.
2. 학습/테스트 데이터를 분리한다.
3. 전처리와 모델을 파이프라인으로 묶어 데이터 누수를 방지한다.
4. 예측 레이블과 확률을 비교한다.
5. 혼동행렬, 정밀도, 재현율, F1-score를 확인한다.
6. 임계값을 바꾸면 오탐과 미탐 사이에 절충이 생긴다는 점을 분석한다.

### 핵심 코드 패턴
```python
from sklearn.linear_model import LogisticRegression
model = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000))
model.fit(X_train, y_train)
pred = model.predict(X_test)
proba = model.predict_proba(X_test)
print(classification_report(y_test, pred, zero_division=0))
```

### 해석
로지스틱 회귀의 출력은 클래스 확률 추정값이다. 계수는 입력 특징의 스케일과 상관관계에 영향을 받으므로 단독으로 인과관계를 의미하지 않는다.

## 3. 결정트리와 세 모델 비교

### 목적
특징별 분기 규칙을 학습하고 KNN·로지스틱 회귀·결정트리의 성능과 해석 가능성을 비교한다.

### 진행 과정
1. 동일한 데이터 분할을 사용해 모델을 비교한다.
2. 결정트리의 깊이, 리프 최소 샘플 수 등을 조절한다.
3. 훈련 성능과 테스트 성능의 차이로 과대적합 여부를 확인한다.
4. 정확도, 정밀도, 재현율, F1-score를 같은 조건에서 계산한다.
5. 트리의 분기 조건과 중요 특징을 해석한다.

### 핵심 코드 패턴
```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

tree = DecisionTreeClassifier(max_depth=5, random_state=42)
tree.fit(X_train, y_train)
pred = tree.predict(X_test)
print({
    "accuracy": accuracy_score(y_test, pred),
    "precision": precision_score(y_test, pred, zero_division=0),
    "recall": recall_score(y_test, pred, zero_division=0),
    "f1": f1_score(y_test, pred, zero_division=0)
})
```

### 해석
트리는 분기 규칙을 설명하기 쉽지만 제한 없이 성장하면 학습 데이터에 과적합될 수 있다. 비교 표의 수치는 실제 노트북 실행 출력에서만 옮겨 적는다.

## 4. 과일 이미지 군집화: K-Means

### 목적
이미지를 수치 배열로 변환한 뒤 레이블 없이 유사한 패턴끼리 묶이는지 살펴본다.

### 진행 과정
1. 이미지 크기와 채널 수를 통일한다.
2. 각 이미지를 1차원 특징 벡터로 변환한다.
3. 평균 이미지와 픽셀 분포를 시각화한다.
4. K-Means를 적용하고 군집별 샘플 수를 확인한다.
5. 군집 중심을 이미지 형태로 복원해 관찰한다.
6. 실제 과일 종류와 군집 번호가 반드시 일치하지 않는 이유를 설명한다.

### 핵심 코드 패턴
```python
from sklearn.cluster import KMeans
km = KMeans(n_clusters=3, random_state=42, n_init=10)
labels = km.fit_predict(flattened_images)
print("군집별 샘플 수:", __import__("numpy").bincount(labels))
```

### 해석
군집 번호 0·1·2는 과일의 정답 레이블이 아니라 알고리즘이 부여한 임의의 번호다. 조명·배경·크기 등 비본질적 이미지 요소가 군집에 영향을 줄 수 있다.

## 5. 이미지 차원 축소: PCA

### 목적
고차원 이미지 특징을 적은 수의 주성분으로 압축하고 정보 보존 정도를 확인한다.

### 진행 과정
1. 이미지 데이터를 2차원 행렬(샘플 × 특징)로 구성한다.
2. PCA를 학습하고 주성분별 설명 분산 비율을 확인한다.
3. 누적 설명 분산으로 필요한 주성분 수를 판단한다.
4. 축소된 특징을 역변환해 원본과 복원 이미지를 비교한다.
5. 차원 축소 전후 분류 성능과 계산 비용을 비교한다.

### 핵심 코드 패턴
```python
from sklearn.decomposition import PCA
pca = PCA(n_components=0.95, random_state=42)
reduced = pca.fit_transform(X_train)
test_reduced = pca.transform(X_test)
print("선택된 주성분 수:", pca.n_components_)
print("설명 분산 합:", pca.explained_variance_ratio_.sum())
```

### 해석
PCA는 분산이 큰 방향을 보존하지만, 분류에 중요한 정보가 항상 큰 분산을 가지는 것은 아니다. 학습/테스트 분리 후 학습 데이터에만 PCA를 적합해야 한다.

## 6. 과일 4종 응용

기존 과일 이미지 예제를 새로운 과일 네 종류로 확장하는 응용 실습이다. 이미지 수집과 크기 정규화, 배열 저장(.npy), 군집화, PCA, 분류 모델 연결을 하나의 흐름으로 점검한다.

- 입력 이미지 수와 클래스별 샘플 수 확인
- 파일 경로, RGB/흑백 변환 및 배열 shape 확인
- 동일 조건에서 군집 중심과 복원 이미지 비교
- 테스트 데이터에 대한 예측과 혼동행렬 기록
- 데이터가 작거나 배경이 일정한 경우 일반화 한계 설명

## 7. 노트북 검증 체크리스트

- [ ] 커널을 재시작하고 전체 셀을 순서대로 실행할 수 있다.
- [ ] 데이터 경로가 하드코딩되어 다른 환경에서 깨지지 않는다.
- [ ] 각 노트북의 실행 출력과 설명이 일치한다.
- [ ] 학습/테스트 데이터 누수가 없다.
- [ ] 그림 제목, 축, 범례가 이해 가능하다.
- [ ] 수치 결과가 없는 경우 임의의 정확도나 점수를 적지 않는다.
- [ ] 대용량·비공개 원본 데이터 및 개인정보가 포함되지 않는다.

## 8. 보안 분야 연결

| 실습 | 보안 활용 가능성 | 유의점 |
|---|---|---|
| KNN | 악성 파일 특징 기반 탐지 | 고차원 거리·오탐 비용 |
| 로지스틱 회귀 | 피싱 위험도 점수화 | 임계값·확률 보정 |
| 결정트리 | 탐지 규칙 해석 | 과대적합·우회 가능성 |
| K-Means | 이상 행위 유형 탐색 | 군집은 정답 레이블이 아님 |
| PCA | 로그·이미지 특징 압축 | 중요한 저분산 신호 손실 |

> 이 문서는 실습 원리와 재현·검증 기준을 정리한 학습 기록이다. 실제 실행 수치와 캡처는 각 제출 노트북을 기준으로 확인한다.
