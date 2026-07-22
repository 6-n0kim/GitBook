# Colab 정형데이터 파일 업로드

### **1. 정형데이터 파일 업로드**

* 1\. Colab 좌측 사이드바의 📁 \[파일] 아이콘을 클릭합니다.
* 2\. 구글 드라이브에서 로컬에 받은 'testdata\_missing.csv' 파일을 좌측 파일 탐색기 창으로 \[드래그 앤 드롭] 해주세요.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```python
target_file = "testdata_missing.csv"

try:
    # CSV 파일 읽기
    origin_df = pd.read_csv(target_file)
    print(f"'{target_file}' 파일을 성공적으로 불러왔습니다!\n")
except FileNotFoundError:
    print(f"오류: '{target_file}' 파일을 찾을 수 없습니다. 파일명이나 경로를 확인해주세요.")
    exit()

print("데이터 정제 및 비식별처리를 시작합니다...\n")
origin_df.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
