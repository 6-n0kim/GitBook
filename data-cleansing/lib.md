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

# 라이브러리 다운로드 및 컬럼 구성 확인

### **2-1. 라이브러리 다운로드**

```python
!pip install pandas==2.2.2 numpy==2.0.2
```

### **2-2. 컬럼 구성 확인**

```python
import pandas as pd
import numpy as np

df.info()
```

\[출력 결과]

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10000 entries, 0 to 9999
Data columns (total 10 columns):
 #   Column     Non-Null Count  Dtype  
---  ------     --------------  -----  
 0   record_id  10000 non-null  object 
 1   이름         10000 non-null  object 
 2   주소         9900 non-null   object 
 3   생년월일       9900 non-null   object 
 4   은행명        9800 non-null   object 
 5   계좌번호       10000 non-null  object 
 6   거래일시       10000 non-null  object 
 7   거래유형       9800 non-null   object 
 8   금액         9700 non-null   float64
 9   거래후잔액      9700 non-null   float64
dtypes: float64(2), object(8)
memory usage: 781.4+ KB
```

이 실습 데이터는 아래 10개 컬럼으로 구성되어 있습니다.

| 컬럼명 | 설명 | 예시 |
|---|---|---|
| record\_id | 레코드 고유번호 | R00001 |
| 이름 | 고객명 | 최예준 |
| 주소 | 주소 | 인천광역시 서초구 강남대로 152 |
| 생년월일 | 생년월일 | 1992-11-07 |
| 은행명 | 은행명 | 국민은행 |
| 계좌번호 | 계좌번호 | 130-21-329258 |
| 거래일시 | 거래 발생 일시 | 2024-12-27 10:13:36 |
| 거래유형 | 입금/출금/이체/송금/결제 | 입금 |
| 금액 | 거래 금액 | 1154556 |
| 거래후잔액 | 거래 후 계좌 잔액 | 2588468 |

***