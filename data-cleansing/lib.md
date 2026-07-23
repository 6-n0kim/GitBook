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

# 필요한 라이브러리 다운로드

### **2-1. 라이브러리 다운로드**

```python
!pip install pandas==2.2.2 numpy==2.0.2
```

### **2-2. 컬럼 구성 확인**

```python
import pandas as pd
import numpy as np

clean_df = origin_df
clean_df.info()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

이 실습 데이터는 아래 10개 컬럼으로 구성되어 있습니다.

<table data-search="false"><thead><tr><th>컬럼명</th><th>설명</th><th>예시</th></tr></thead><tbody><tr><td>record_id</td><td>레코드 고유번호</td><td>R00001</td></tr><tr><td>이름</td><td>고객명</td><td>최예준</td></tr><tr><td>주소</td><td>주소</td><td>인천광역시 서초구 강남대로 152</td></tr><tr><td>생년월일</td><td>생년월일</td><td>1992-11-07</td></tr><tr><td>은행명</td><td>은행명</td><td>국민은행</td></tr><tr><td>계좌번호</td><td>계좌번호</td><td>130-21-329258</td></tr><tr><td>거래일시</td><td>거래 발생 일시</td><td>2024-12-27 10:13:36</td></tr><tr><td>거래유형</td><td>입금/출금/이체/송금/결제</td><td>입금</td></tr><tr><td>금액</td><td>거래 금액</td><td>1154556</td></tr><tr><td>거래후잔액</td><td>거래 후 계좌 잔액</td><td>2588468</td></tr></tbody></table>

***
