---
icon: file-lines
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

# Colab 텍스트 파일 업로드

### 1. **텍스트 업로드**

* 1\. Colab 좌측 사이드바의 📁 \[파일] 아이콘을 클릭합니다.
* 2-1. 구글 드라이브에서 로컬에 받은 'test\_01.txt' 파일(또는 02, 03.txt)을 좌측 파일 탐색기 창으로 \[드래그 앤 드롭] 해주세요.
* 2-2. \[드래그 앤 드롭]이 안된다면 Colab 파일 업로드 UI를 통해, 실습에 사용할 파일을 업로드합니다.

<figure><img src="../.gitbook/assets/텍스트업로드.png" alt=""><figcaption></figcaption></figure>

```python
from google.colab import files
import os
from google.colab import files

# 실습 기본 대상 파일명 (text_01.txt ~ 03.txt 중 선택)
target_file = 'text_01.txt'

if not os.path.exists(target_file):
  # Colab 파일 업로드 UI 띄우기
  upload = files.upload()

  # 업로드된 첫 번째 파일 경로
  txt_path = list(upload.keys())[0]
else:
  txt_path = target_file

print(f"'{txt_path}' 텍스트 로드 완료!")
# 텍스트 읽기
with open(txt_path, encoding='utf-8') as f:
  text = f.read()

print("원본 텍스트:\n", text)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/텍스트로드.png" alt=""><figcaption></figcaption></figure>
