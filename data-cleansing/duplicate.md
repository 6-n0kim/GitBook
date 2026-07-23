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

### 4. 중복 데이터 확인

중복 데이터는 분석 왜곡과 비즈니스 오류를 방지하고, 시스템 비용을 절감하여 데이터의 신뢰성을 확보하기 위해 삭제합니다.

```python
# 전체 행 기준 완전 중복 확인
print("완전 중복 행 수:", clean_df.duplicated().sum())

# 제거될 중복 행 출력
print("--- 제거될 완전 중복 행 ---")
print(clean_df[clean_df.duplicated()])

# 중복 제거
clean_df = clean_df.drop_duplicates()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
