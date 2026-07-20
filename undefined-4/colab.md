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

# Colab 텍스트 파일 업로드

### 1. **텍스트 업로드**

* Colab 파일 업로드 UI를 통해, 실습에 사용할 텍스트 파일을 업로드합니다.

```python
from google.colab import files

# Colab 파일 업로드 UI 띄우기
upload = files.upload()

# 업로드된 첫 번째 파일 경로
txt_path = list(upload.keys())[0]
print("file_name:", txt_path)

# 텍스트 읽기
with open(txt_path, encoding='utf-8') as f:
    text = f.read()
    
print("원본 텍스트:\n", text)
```

\[출력 결과]
