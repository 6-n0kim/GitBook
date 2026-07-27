---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# 합성 적용 - 2. 순차적 CART 합성

### 2. 순차적 CART 합성

앞에서 이미 합성한 컬럼을 "조건(예측 변수)"으로 써서 다음 컬럼을 하나씩 합성합니다.

> 순서와 예측 변수 설정 근거
>
> * 연령대: 주소(지역)에 따라 인구 구성이 다를 수 있어 주소를 조건으로.
> * 거래유형: 주소+연령대(인구통계)에 따라 씀씀이 패턴이 다를 수 있어 그 둘을 조건으로.
> * 은행명: 지역/연령대/거래유형에 따라 주 거래은행이 달라질 수 있어 그 셋을 조건으로.
> * 금액: 지역+연령대+거래유형+은행에서 나온 거래인지에 따라 규모가 다를 수 있어서.
> * 거래후잔액: 금액(방금 나간/들어온 돈)과 직접 관련이 있으니 금액까지 포함해서 조건으로.

```python
# ── 연령대 → 거래유형 → 은행명 → 금액 → 거래후잔액: 순서대로 CART 합성 ──
SEQUENTIAL_PLAN = [
    ('연령대', ['주소']),
    ('거래유형', ['주소', '연령대']),
    ('은행명', ['주소', '연령대', '거래유형']),
    ('금액', ['주소', '연령대', '거래유형', '은행명']),
    ('거래후잔액', ['주소', '연령대', '거래유형', '은행명', '금액']),
]

for target_col, predictor_cols in SEQUENTIAL_PLAN:
    X_real, X_synth = encode_predictors(process_df, synth_df, predictor_cols)
    synth_df[target_col] = cart_synthesize(X_real, process_df[target_col], X_synth, kind='categorical')
    
synth_df.head(10)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>
