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

# Colab 이미지 파일 업로드

### 1. **이미지 업로드**

* Colab 파일 업로드 UI를 통해, 실습에 사용할 이미지 파일을 업로드합니다.

```python
from google.colab import files
from PIL import Image, ImageDraw, ImageFont, ImageFilter
import matplotlib.pyplot as plt

# Colab 파일 업로드 UI 띄우기
upload = files.upload()

# 업로드된 첫 번째 파일 경로
img_path  = list(upload.keys())[0]

# 이미지 로드 & 출력
image = Image.open(img_path).convert("RGB")
plt.imshow(image)
plt.axis('off')
plt.show()
```
