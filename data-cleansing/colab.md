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

# Colab 정형데이터 파일 업로드

### **2. 정형데이터 파일 업로드**

* 1\. Colab 좌측 사이드바의 📁 \[파일] 아이콘을 클릭합니다.
* 2-1. 구글 드라이브에서 로컬에 받은 'testdata\_missing.csv' 파일을 좌측 파일 탐색기 창으로 \[드래그 앤 드롭] 해주세요.
* 2-2. \[드래그 앤 드롭]이 안된다면 Colab 파일 업로드 UI를 통해, 실습에 사용할 파일을 업로드합니다.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```python
import pandas as pd
import numpy as np
from google.colab import files

target_file = "testdata_missing.csv"

try:
  # CSV 파일 읽기
  origin_df = pd.read_csv(target_file)

except FileNotFoundError:
  # Colab 파일 업로드 UI 띄우기
  upload = files.upload()

  # 업로드된 첫 번째 파일 경로
  target_file = list(upload.keys())[0]

  # CSV 파일 읽기
  origin_df = pd.read_csv(target_file)

print(f"'{target_file}' 파일을 성공적으로 불러왔습니다!")

print(f"총 행 - {origin_df.shape[0]}")
print(f"총 열 - {origin_df.shape[1]}")
pd.set_option('display.float_format', '{:.0f}'.format)
clean_df = origin_df.copy()

display(clean_df.head())
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

***

이 실습 데이터는 아래 10개 컬럼으로 구성되어 있습니다.

<table data-search="false"><thead><tr><th>컬럼명</th><th>설명</th><th>예시</th></tr></thead><tbody><tr><td>record_id</td><td>레코드 고유번호</td><td>1</td></tr><tr><td>이름</td><td>고객명</td><td>최예준</td></tr><tr><td>주소</td><td>주소</td><td>성남시 분당구 중앙로 115</td></tr><tr><td>생년월일</td><td>생년월일</td><td>1992-11-07</td></tr><tr><td>은행명</td><td>은행명</td><td>국민은행</td></tr><tr><td>계좌번호</td><td>계좌번호</td><td>130-21-329258</td></tr><tr><td>거래일시</td><td>거래 발생 일시</td><td>2024-12-27 10:13:36</td></tr><tr><td>거래유형</td><td>입금/출금/이체/송금/결제</td><td>입금</td></tr><tr><td>금액</td><td>거래 금액</td><td>1154556</td></tr><tr><td>거래후잔액</td><td>거래 후 계좌 잔액</td><td>2588468</td></tr></tbody></table>
