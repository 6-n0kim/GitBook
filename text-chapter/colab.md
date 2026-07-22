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
* 2\. 구글 드라이브에서 로컬에 받은 'test\_01.txt' 파일(또는 02, 03.txt)을 좌측 파일 탐색기 창으로 \[드래그 앤 드롭] 해주세요.

<figure><img src="../.gitbook/assets/텍스트업로드.png" alt=""><figcaption></figcaption></figure>

```python
import os

# 실습 기본 대상 파일명 (text_01.txt ~ 03.txt 중 선택)
target_file = 'text_01.txt'

if not os.path.exists(target_file):
  print(f"❌ '{target_file}' 파일이 현재 Colab 환경에 없습니다. 업로드 가이드를 확인 후 다시 시도해 주세요.")
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
