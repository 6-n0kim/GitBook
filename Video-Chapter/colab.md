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

# Colab 비디오 파일 업로드

### 1. **비디오 업로드**

* Colab 파일 업로드 UI를 통해, 실습에 사용할 비디오 파일을 업로드합니다.

```python
from google.colab import files
from PIL import Image, ImageDraw, ImageFont, ImageFilter
import cv2
import matplotlib.pyplot as plt

# Colab 파일 업로드 UI 띄우기
upload = files.upload()

# 업로드된 첫 번째 파일 경로
video_path  = list(upload.keys())[0]

# 비디오 열기
cap = cv2.VideoCapture(video_path)

# 첫 프레임 추출
ret, frame = cap.read()
cap.release()

# 출력
franme_rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
plt.figure(figsize=(8, 6))
plt.imshow(franme_rgb)
plt.axis('off')
plt.show
```
