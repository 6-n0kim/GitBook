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

# 전체 데이터 비교

```python
print("초기 데이터")
display(origin_df.head())
print("정제 데이터")
display(clean_df.head())
print("비식별처리 데이터")
display(process_df.head())
print("합성 데이터")
display(synth_df.head())
```

\[출력 결과]

<figure><img src=".gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>
