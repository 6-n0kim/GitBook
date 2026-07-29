---
icon: image
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

* 1\. Colab 좌측 사이드바의 📁 \[파일] 아이콘을 클릭합니다.
* 2-1. 구글 드라이브에서 로컬에 받은 'test\_001.jpg' 파일 또는 확장자가 png인 test\_(숫자)을 좌측 파일 탐색기 창으로 \[드래그 앤 드롭] 해주세요.
* 2-2. \[드래그 앤 드롭]이 안된다면 Colab 파일 업로드 UI를 통해, 실습에 사용할 파일을 업로드합니다.

<figure><img src="../.gitbook/assets/이미지업로드.png" alt=""><figcaption></figcaption></figure>

```python
import os
from PIL import Image, ImageDraw, ImageFont, ImageFilter
import matplotlib.pyplot as plt
from google.colab import files

# 실습 기본 대상 파일명 (test_001.jpg 또는 test_(숫자).png)
target_file = 'test_041.png'

if not os.path.exists(target_file):
  # Colab 파일 업로드 UI 띄우기
  upload = files.upload()

  # 업로드된 첫 번째 파일 경로
  img_path = list(upload.keys())[0]

else:
  img_path = target_file

print(f"'{img_path}' 이미지 로드 완료!")
# 이미지 로드 & 출력
image = Image.open(img_path).convert("RGB")
plt.imshow(image)
plt.axis('off')
plt.show()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image_111.png" alt=""><figcaption></figcaption></figure>
