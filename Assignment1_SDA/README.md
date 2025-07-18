# 🧠 SDA Assignment 1: Numpy 배열, 슬라이싱, 거리계산 실습

본 프로젝트는 `Python`과 `Numpy`를 활용하여 배열의 구조 차이와 슬라이싱의 원리를 이해하고,  
대규모 데이터 행렬에서 샘플 간 거리 계산을 **for-loop vs. 벡터화(vectorization)** 방식으로 비교 분석한 과제입니다.

---

## 📌 실습 환경 설정

- Python Version: **3.8.18**
- Anaconda: `conda create -n sda2024 python=3.8.18`
- JupyterLab 환경에서 실행
---

### 📁 파일 구성
Assignment1_SDA/
├── Assignment1_SDA.ipynb   # Jupyter 실습 코드
├── Assignment1_SDA.pdf     # 과제 보고서
├── README.md               # 현재 파일


## 🔍 주요 과제 내용

### 1. 배열 차원의 이해: (4,) vs (1,4), (3,) vs (3,1)

- 단일 인덱싱 (`a[1, :]`, `a[:, 1]`) → 차원 축소 → 1D 벡터
- 슬라이싱 (`a[1:2, :]`, `a[:, 1:2]`) → 차원 유지 → 2D 배열
- 차원이 달라지면 행렬 연산 시 shape mismatch 오류가 발생할 수 있으므로 주의 필요

📌 핵심 요약:

| 표현 | 결과 shape |
|------|-------------|
| `a[1, :]` | `(n,)` → 1차원 |
| `a[1:2, :]` | `(1, n)` → 2차원 |
| `a[:, 1]` | `(m,)` → 1차원 |
| `a[:, 1:2]` | `(m, 1)` → 2차원 |

---

### 2. 랜덤 데이터 매트릭스 생성 및 거리 계산

- 크기: `1000 x 1000` 매트릭스 생성 (실제 계산은 `100 x 100`으로 수행)
- 각 `i`번째 샘플과 `j`번째 샘플 간의 거리 계산 수행

#### ✅ 방식 1: For-loop 사용
```python
for i in range(n):
    for j in range(n):
        dist[i, j] = np.linalg.norm(X[i] - X[j])

###  ✅ 방식 2: Vectorize 사용
dists = np.sqrt(
    np.sum(X**2, axis=1)[:, np.newaxis] +
    np.sum(X**2, axis=1)[np.newaxis, :] -
    2 * np.dot(X, X.T)
)

### 수행시간 시각화

<img width="836" height="491" alt="image" src="https://github.com/user-attachments/assets/c772e6a8-882f-41fe-b4d0-f4664217c841" />


