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

```python
# 거래일시를 datetime 타입으로 변환
clean_df['거래일시'] = pd.to_datetime(clean_df['거래일시'])

# 생년월일을 datetime 타입으로 변환 (③에서 형식 통일 후)
# 남아있는 결측치(NaN)는 자동으로 NaT(Not a Time)로 변환되어 별도 처리가 필요 없습니다.
clean_df['생년월일'] = pd.to_datetime(clean_df['생년월일'])

# 계좌번호는 숫자로 변환하지 않고 문자열(str)로 유지 (앞자리 0 손실 방지, 하이픈 포함)
clean_df['계좌번호'] = clean_df['계좌번호'].astype(str)

clean_df.info()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

### **정제 결과 확인**

```python
clean_df.head(10)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
