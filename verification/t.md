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

# T-근접성

### 3. T-근접성(T-closeness)

* `분포거리`는 동질집합 안 `거래유형` 비율이 전체 `거래유형` 비율과 얼마나 다른지를 0\~1 사이 값(변동거리)으로 나타냅니다.
* &#x20;ex) 분포거리 0.4면 그 그룹은 전체보다 특정 거래유형에 40%p만큼 더 쏠려있다는 뜻이다.

```python
global_dist = process_df[sensitive_col].value_counts(normalize=True)

def variational_distance(s):
    group_dist = s.value_counts(normalize=True).reindex(global_dist.index, fill_value=0)
    return 0.5 * (group_dist - global_dist).abs().sum()

closeness_df = process_df.groupby(qi_cols)[sensitive_col].apply(variational_distance).reset_index(name='분포거리')

# 상위 15개 (전체 분포와 가장 차이가 큰 취약한 동질집합)
top_15_distance = closeness_df.sort_values('분포거리', ascending=False).head(15).reset_index(drop=True)
display(top_15_distance)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

```python
# 변동거리 0.2 초과면 해당 그룹의 거래유형 비율이 전체와 20%p 이상 벌어졌다고 본다.
t_target = 0.2
vulnerable_t = closeness_df[closeness_df['분포거리'] > t_target]
print(f"t={t_target} 기준 취약 동질집합 개수:", len(vulnerable_t))
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>
