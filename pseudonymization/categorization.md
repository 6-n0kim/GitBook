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

# 범주화

### 5. 범주화처리&#x20;

구체적인 수치나 단일값을 일정한 범위나 구간으로 묶어서 대표값으로 표현합니다.

```python
# ── record_id: 순차 코드 매핑 방식 ──
unique_ids = process_df['record_id'].unique()
id_map = {id: f"R{i+1:05d}" for i, id in enumerate(unique_ids)}

process_df['record_id'] = process_df['record_id'].map(id_map)

print("고유 id 개수:", process_df['record_id'].nunique(), "/ 매핑 테이블 크기:", len(id_map))
process_df[['이름', 'record_id']].head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
