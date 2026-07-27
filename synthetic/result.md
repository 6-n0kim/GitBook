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

# 합성 결과 확인 및 검증

### **5-1.**  합성 결과 확인

합성데이터와 원본데이터의 분포 특성이 얼마나 유사한지, 같은 목표를 달성할 수 있는지 등을 검증합니다.

```python
import matplotlib.pyplot as plt
import numpy as np

def plot_distribution_compare(ax, real_series, synth_series, title, sort='freq'):
    if sort == 'age':
        idx = sorted(real_series.unique(), key=lambda x: int(x.replace('대', '')))
        real_dist = real_series.value_counts(normalize=True).reindex(idx, fill_value=0)
    else:
        real_dist = real_series.value_counts(normalize=True)

    synth_dist = synth_series.value_counts(normalize=True).reindex(real_dist.index, fill_value=0)

    x = np.arange(len(real_dist))
    width = 0.35
    ax.bar(x - width/2, real_dist.values, width, label='실제', color='tab:blue')
    ax.bar(x + width/2, synth_dist.values, width, label='합성', color='tab:orange')
    ax.set_xticks(x)
    ax.set_xticklabels(real_dist.index, rotation=45, ha='right')
    ax.set_ylabel('비율')
    ax.set_title(title)
    ax.legend()

cols_to_check = [('주소', 'freq'), ('연령대', 'age'), ('거래유형', 'freq'), ('은행명', 'freq')]
fig, axes = plt.subplots(2, 2, figsize=(13, 9))

for ax, (col, sort_mode) in zip(axes.flat, cols_to_check):
    plot_distribution_compare(ax, process_df[col], synth_df[col], col, sort=sort_mode)

fig.suptitle('실제 vs 합성 데이터 분포 비교', fontsize=14)
plt.tight_layout()
plt.show()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

### 5-2. 안전성 검증

생성된 합성데이터를 통해 원본데이터 내 개인이 식별될 가능성이 있는지 검증합니다.

정형 합성데이터의 안전성 지표로는 아래와 같이 있습니다.&#x20;

* **구별 위험도:** 원본과 같은 레코드가 존재할 위험
* **연결 위험도:** 준식별자를 통해 민감정보를 유추할 위험, CAP
* **추론 위험도:** 유사한 레코드로 개인정보를 추론할 위험

이 실습에서는 세 지표 중 계산이 가장 직관적이고, Step 4에서 사용한 synthpop(CART) 방식이 특히 취약한 지점이기 때문에 이 중 **구별 위험도**만 측정합니다.&#x20;

> record\_id와 계좌번호는 합성 단계에서 새로 부여한 값이라 비교 대상에서 제외하고 나머지 컬럼이 전부 일치하는 행이 있는지 봅니다.

```python
compare_cols = [c for c in process_df.columns if c not in ['record_id', '계좌번호']]

# 합성 레코드마다 원본에 같은 레코드가 있으면 1, 없으면 0 → 평균
real_keys = set(map(tuple, process_df[compare_cols].itertuples(index=False, name=None)))
is_matched = synth_df[compare_cols].apply(tuple, axis=1).isin(real_keys)

single_out_risk = is_matched.mean()
print(f"구별 위험도(Single out risk): {single_out_risk:.4f} ({is_matched.sum()}건 / {len(synth_df)}건)")
display(synth_df[is_matched].head())
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>
