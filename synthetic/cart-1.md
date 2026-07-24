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

# 합성 적용 - 1. 무작위 재추출

### 1. 무작위 재추출

제일 먼저 합성하는 컬럼은 조건으로 쓸 '이전 컬럼'이 없습니. 그래서 CART 대신, 실제 `주소` 컬럼의 값을 통째로 무작위 재추출(복원추출)해서 채웁니다.

> 실제 데이터의 주소 분포(어느 지역이 몇 %인지)는 그대로 재현되지만, 특정 합성 행 하나가 실제 몇 번째 사람과 짝지어지지는 않습니다.

```python
# ── 주소: 초기 컬럼, 실제 분포에서 무작위 재추출 ──
n = len(process_df)
rng = np.random.default_rng(SYNTH_SEED)
synth_df = pd.DataFrame(index=range(n))
synth_df['주소'] = rng.choice(process_df['주소'].values, size=n, replace=True)

synth_df.head(10)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>
