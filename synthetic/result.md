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

# 합성 결과 확인

### 4. 합성 결과 확인

합성 데이터가 실제 데이터의 컬럼별 분포를 얼마나 잘 재현했는지(통계적 유사성)를 확인하고, 실제 레코드와 완전히 일치하는 합성 행이 있는지(재식별 위험)를 함께 점검합니다.

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
