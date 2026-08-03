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

# 정제 결과

### 기존 데이터

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

### 정제 전체 결과

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

```python
final_file = '000_data.csv'
000_df.to_csv(final_file, index=False, encoding='utf-8-sig')
```
