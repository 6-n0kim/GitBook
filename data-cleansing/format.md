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

# 데이터 타입 변환

### 7. 데이터 타입 변환

데이터 타입을 해당 데이터에 맞는 타입으로 변환합니다.

```python
# 생년월일, 거래일시를 datetime 타입으로 변환
clean_df['거래일시'] = pd.to_datetime(clean_df['거래일시'])
clean_df['생년월일'] = pd.to_datetime(clean_df['생년월일'])

# 계좌번호는 숫자로 변환하지 않고 문자열(str)로 유지
clean_df['계좌번호'] = clean_df['계좌번호'].astype(str)

clean_df.info()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>
