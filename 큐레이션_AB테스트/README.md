🎯 큐레이션 A/B 테스트 (컬리 ― 추석 선물 카테고리)

목적:
상품 특징 중심(A안)보다 대상/상황 중심(B안) 큐레이션이 사용자 맥락에 더 잘 맞아
클릭률(CTR), 구매전환율(CVR), 세션당 매출(RPS) 을 개선하는지 검증합니다.

🧩 배경 & 가설

배경: 현재 ‘추석 선물’ 카테고리는 컬리온리/실용만점/프리미엄/다다익선 등 상품 특징 중심.
실제 사용자는 “누구에게/어떤 상황에 선물할지”부터 고민 → 맥락 기반 발견이 중요.

가설: 대상/상황 중심 큐레이션(B안)이 A안 대비 **CTR↑, CVR↑**로 퍼널을 개선한다.

🧪 A/B 테스트 설계

A (Control): 기존 테마 유지
예) 컬리온리, 실용만점, 프리미엄 선물세트, 다다익선

B (Experiment): 대상/상황 중심 테마
예) #감사한 부모님께 · #센스있는 동료에게 · #마음을 전할 은사님께 · #부담없는 실속선물

트래픽 배분: 50:50 (seed 고정)

분석 단위: 세션(session)

🏗️ 데이터 생성 & 노이즈 주입 (시뮬레이션)

현실 로그의 지저분함을 반영하기 위해 의도적으로 노이즈를 넣었습니다.

generate_raw_data()
최근 2주 timestamp, device, page_views, clicked_curation, purchase_amount,
purchased_flag, abandoned_at_payment 생성 (그룹별 CTR·CVR·Bounce·AOV 기대치 반영)

generate_noisy_data()

중복 세션(예: 5%)

device 오타: moblie, deskotp (≈10%)

구매금액 이상치: -5,000, 10,000,000, 20,000,000 (≈1%)

미래 시각 timestamp (≈0.2%)

클릭 결측(NaN) (≈1%)

df_noisy = generate_noisy_data(num_sessions=10000, seed=42)
df_noisy.to_csv("data/curation_ab_test_noisy.csv", index=False)

🧹 전처리 파이프라인 (왜 이렇게 했나)

중복 제거: drop_duplicates(['session_id'])

오타 정규화: moblie→mobile, deskotp→desktop

금액 이상치 제거: <0 또는 > 1,000,000원 → NaN

시간 정제: timestamp 파싱 → 미래 시각 행 제거

클릭 결측 처리: clicked_curation NaN → 0(미클릭 간주)

파생 변수

converted = (purchase_amount_filled>0)

bounced = (page_views==1)

abandon_pay = isna(purchase_amount) (결제 단계 이탈로 정의)

revenue = purchase_amount_filled (NaN→0)

이유: 실제 로그에는 중복/오타/결측/이상치/미래시점 등 불량이 상존.
실무형 전처리로 측정 편향을 최소화하고 재현 가능한 분석을 보장합니다.

📏 핵심 지표

Primary:

CTR(큐레이션 클릭률), CVR(카테고리 구매 전환율)

Secondary:

AOV(구매자 1인당 평균 결제금액), Bounce(즉시 이탈률)

Reference:

RPS(세션당 매출 = 총매출/세션수), Payment Abandon(결제 단계 이탈률)

🧠 분석 방법 (선택 근거)

비율 지표(CTR/CVR/Bounce/Abandon)

두 비율 z-검정(proportions_ztest), Wilson CI

다중비교 보정: Holm–Bonferroni (FWER 5% 보장, Bonferroni보다 파워↑)

연속 지표(AOV/RPS)

Welch t-test(equal_var=False): 등분산 가정 회피, 분산 이질성에 안전

부트스트랩 CI(평균): 분포 가정 최소화(RPS처럼 0이 많은 분포에 유리)

대용량에서 정규성/등분산 사전검정은 과민하며, 사전검정→분기 전략은 1종 오류율 왜곡 가능.
⇒ Welch + 부트스트랩 조합이 안전한 실무 표준.
