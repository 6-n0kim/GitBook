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

# 중복 데이터 확인

```python
# 전체 행 기준 완전 중복 확인
print("완전 중복 행 수:", df.duplicated().sum())

# 특정 컬럼(예: record_id) 기준 중복 확인 - 고유 식별자는 중복되면 안 됨
print("record_id 중복 수:", df['record_id'].duplicated().sum())

# 중복 제거 (필요 시)
# df = df.drop_duplicates()
```

\[출력 결과]

```
완전 중복 행 수: 0
record_id 중복 수: 0
```
