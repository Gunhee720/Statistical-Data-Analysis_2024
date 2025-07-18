# 📊 SDA Assignment 2: 분포 비교, 통계량 계산, 시각화 및 연봉 분석

본 프로젝트는 Python 기반 통계 실습으로,  
- 서로 다른 분포(정규분포 vs 지수분포)를 시각화하고  
- 다양한 통계량(mean, median 등)을 벡터화 vs 반복문 방식으로 비교하며,  
- 실제 데이터(mba salary)를 활용해 팀/포지션/능력치별 연봉 차이를 분석합니다.

---

## 🔧 실습 환경

- Python 3.x
- Jupyter Notebook (Q1_sda.ipynb, Q2_sda.ipynb, Q3_sda.ipynb)
- Pandas, Numpy, Matplotlib, Seaborn 등 사용

---

## 📁 과제 구성

| 파일명 | 설명 |
|--------|------|
| `Q1_sda.ipynb` | 정규/지수 분포 시뮬레이션, 통계량 계산 |
| `Q2_sda.ipynb` | 시각화 실습 (박스플롯, 범례 수정 등) |
| `Q3_sda.ipynb` | MBA salary 데이터 기반 통계 분석 |
| `Assignment_SDA2.pdf` | 과제 보고서 (설명 + 시각화 결과 포함) |

---

## ✅ Q1: 정규 vs 지수 분포 시뮬레이션 및 통계량 비교

- 정규분포와 지수분포에서 각 100,000개의 샘플 생성
- 분포 시각화 (히스토그램)
- 주요 통계량 계산

### 📊 비교한 통계량:
- Mean
- Median
- Variance
- MAD
- Skewness
- Kurtosis
- IQR

### 🚀 성능 비교 결과:

| 방식 | 수행 시간 |
|------|------------|
| 반복문 기반 | 약 **712ms** |
| 벡터화 기반 | 약 **44ms** ✅ 빠름 |

<img width="971" height="470" alt="image" src="https://github.com/user-attachments/assets/5909a2b7-4bed-43c7-925e-e08ded01b93f" />


---

## ✅ Q2: 시각화 실습

- 시각화 강의자료의 박스플롯을 재현
- 범례 오류 해결 (GPT 활용하여 `get_legend_handles_labels()`로 수정)
- Outlier 스타일 개선

<img width="603" height="425" alt="image" src="https://github.com/user-attachments/assets/590c7757-a615-4767-836a-e3a258df97ce" />

<img width="619" height="438" alt="image" src="https://github.com/user-attachments/assets/89afce4f-913e-4cf2-b2a9-21149508df13" />

---

## ✅ Q3: MBA 연봉 데이터 분석 (2017~2018 시즌 기준)

### 📌 질문 1: 팀별 연봉 분포와 분포가 2개인 이유?

- 팀별 연봉 분포를 barplot으로 시각화  
- 고액 연봉자가 특정 팀에 몰려 분포가 2개로 나뉨  
- **색상을 연봉값 기준으로 조절해 시각적 직관성 향상 (GPT 활용)**

---

### 📌 질문 2: 포지션별 연봉 차이는 통계적으로 유의미한가?

- 포지션 → 범주형 독립변수  
- 연봉 → 연속형 종속변수

→ **ANOVA 분석 수행**

F-statistic: 0.5387
p-value: 0.7074


📌 결론: **p-value > 0.05 → 유의미한 차이 없음**  
즉, 포지션별 연봉은 통계적으로 차이가 있다고 보기 어려움

---

### 📌 질문 3: PER와 연봉의 상관 관계는?

- 2014~2017 시즌 데이터 기반으로 선수별 평균 PER 계산  
- 연봉 데이터와 merge하여 **상관계수 계산**

📈 **상관계수 ≈ 0.46** → **PER와 연봉은 양의 상관 관계**

<img width="971" height="566" alt="image" src="https://github.com/user-attachments/assets/3d2c01d8-6d2f-4a4d-855d-f6a5a530da47" />

<img width="818" height="579" alt="image" src="https://github.com/user-attachments/assets/0cfccf99-7088-47f0-b6ee-1d6ba9ffd98a" />

---

## 🧠 총평

- 벡터화 기법의 속도 차이를 직접 체험
- GPT를 활용한 시각화 오류 해결 및 코드 보완
- 실제 데이터를 기반으로 한 통계적 분석 경험

> “포지션은 연봉에 큰 영향을 미치지 않지만, 능력치(PER)는 확실히 영향력이 있음을 확인했습니다.”

---


