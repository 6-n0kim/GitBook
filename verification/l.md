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

# L-다양성

### 2. L-다양성(L-diversity)

* `민감정보다양성`은 동질집합 하나 안에 존재하는 `거래유형`의 고유값 개수입니다.&#x20;
* ex) "고양시+70대" 그룹에 입금·출금·이체만 있다면 다양성은 3입니다.

```python
sensitive_col = '거래유형'

diversity_df = process_df.groupby(qi_cols)[sensitive_col].nunique().reset_index(name='민감정보다양성')

# 하위 15개 (다양성이 가장 낮은 취약한 동질집합)
bottom_15_diversity = diversity_df.sort_values('민감정보다양성').head(15).reset_index(drop=True)
display(bottom_15_diversity)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

```python
# 거래유형처럼 카테고리 수가 적은(5종) 민감정보는 최소 3종 이상 섞여있어야 안전하다고 본다.
l_target = 3
vulnerable_l = diversity_df[diversity_df['민감정보다양성'] < l_target]
print(f"l={l_target} 기준 취약 동질집합 개수:", len(vulnerable_l))
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>
