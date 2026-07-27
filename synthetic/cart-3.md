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

# 합성 적용 - 3. 수치형 CART 합성

### 3. 수치형 CART 합성

방식은 2번과 같지만 목표값이 날짜라, 초 단위 숫자로 바꿔 regression tree(회귀 트리)를 사용합니다. 합성 후 다시 datetime으로 복원합니다.

> datetime의 기준을"1970-01-01"로 잡은 이유
>
> `datetime64` 타입 자체가 내부적으로 "1970-01-01부터 몇 \[나노초/마이크로초/초] 지났는지"로 저장됩니다. 그래서 `EPOCH`을 1970-01-01로 잡으면, `real_ts` 값이 곧 **표준 유닉스 타임스탬프**(다른 언어·DB에서도 다 쓰는 그 숫자)가 됩니다.

```python
# ── 거래일시: 초 단위 수치로 바꿔서 합성 후 다시 datetime으로 복원 ──
predictor_cols = ['주소', '연령대', '거래유형', '은행명', '금액', '거래후잔액']
X_real, X_synth = encode_predictors(process_df, synth_df, predictor_cols)

EPOCH = pd.Timestamp("1970-01-01")
real_ts = (process_df['거래일시'] - EPOCH) // pd.Timedelta('1s')
synth_ts = cart_synthesize(X_real, real_ts, X_synth, kind='numeric')
synth_df['거래일시'] = EPOCH + pd.to_timedelta(synth_ts, unit='s')

synth_df.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>
