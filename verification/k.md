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

# K-익명성

### 1. K-익명성(K-anonymity)

* `동질집합크기`는 동질집합 **하나** 안에 들어있는 사람 수를 의미합니다.&#x20;
* ex) "고양시+70대" 그룹의  동질집합크기가 28이라면, 그 그룹의 사람 수는 28명입니다.

```python
pd.reset_option('display.float_format')

qi_cols = ['주소', '연령대']

anonymity_df = process_df.groupby(qi_cols).size().reset_index(name='동질집합크기')

total_df = pd.DataFrame({
    '총 동질집합개수': [len(anonymity_df)]
})

print("총 동질집합개수")
display(total_df)

# 하위 15개 (익명성이 가장 낮은 취약한 동질집합)
bottom_15_df = anonymity_df.sort_values('동질집합크기').head(15).reset_index(drop=True)

display(bottom_15_df)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

```python
# 민감 정보가 포함된 금융 데이터는 k=5를 기준으로 k-익명성을 검증한다.
k_target = 5
vulnerable = group_sizes[group_sizes['동질집합크기'] < k_target]
print(f"k={k_target} 기준 취약 동질집합 개수:", len(vulnerable))
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>
