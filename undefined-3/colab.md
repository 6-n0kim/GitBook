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

# Colab 음성 파일 업로드

### 1. **오디오 업로드**

* Colab 파일 업로드 UI를 통해, 실습에 사용할 음성 파일을 업로드합니다.

```python
from google.colab import files

# Colab 파일 업로드 UI 띄우기
upload = files.upload()

# 업로드된 첫 번째 파일 경로
audio_path = list(upload.keys())[0]

print("file_name:", audio_path)
```
