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

# 합성 적용 - 3. 수치형 변환

### 3-1. 날짜형 데이터 변환

tree는 숫자만 다룰 수 있어서, datetime을 "1970-01-01부터 몇 초 지났는지"로 바꿔서 처리합니다.

> datetime의 기준을"1970-01-01"로 잡은 이유
>
> `datetime64` 타입 자체가 내부적으로 "1970-01-01부터 몇 \[나노초/마이크로초/초] 지났는지"로 저장됩니다. 그래서 `EPOCH`을 1970-01-01로 잡으면, `real_ts` 값이 곧 **표준 유닉스 타임스탬프**(다른 언어·DB에서도 다 쓰는 그 숫자)가 됩니다.

```python
# ── 거래일시: 초 단위 수치로 바꿔서 회귀나무로 합성 후 다시 datetime으로 복원 ──
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

#### 이름/계좌번호/record\_id는 CART로 모델링할 관계가 없어 별도 처리&#x20;

```python
synth_df['이름'] = rng.choice(process_df['이름'].values, size=n, replace=True)
synth_df['계좌번호'] = [hashlib.sha256(f"synthetic_{i}_{SYNTH_SEED}".encode()).hexdigest()[:16] for i in range(n)]
synth_df['record_id'] = [f"R{i+1:05d}" for i in range(n)]

synth_df = synth_df[process_df.columns.tolist()]
synth_df.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>
