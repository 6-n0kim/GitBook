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

# 바인딩 박스 좌표 구하기

### **3. 바운딩 박스 좌표 생성**

* 탐지된 얼굴의 박스 좌표 `(x0, y0, x1, y1)`를 **Python 리스트 형태로 정리**합니다.
* 이 좌표 정보는 이후의 블러 처리나 마스킹 처리에 **재사용 가능하도록 저장**하는 단계입니다.

```python
import cv2
import numpy as np

# PIL→OpenCV 변환 (RGB→BGR)
np_img = cv2.cvtColor(np.array(image), cv2.COLOR_RGB2BGR)

# (한글이름, 표시태그, 바운딩박스리스트, 박스색상BGR)
items = [
    ("얼굴",   "FACE",  face_boxes,  (0, 0, 255)),
    ("번호판", "PLATE", plate_boxes, (255, 0, 0)),
]

# 각 항목(얼굴/번호판)을 순회하며 출력+그리기
for ko_label, en_tag, boxes, color in items:
    print(f"{ko_label} 바운딩 박스:") # 표제 줄 출력
    if not boxes:
        print(f" {ko_label}이 탐지되지 않았습니다.")
        continue

    # 각 바운딩 박스를 순회
    for i, (x0, y0, x1, y1) in enumerate(boxes):
        print(f"{ko_label} {i}: (x0={x0}, y0={y0}, x1={x1}, y1={y1})")  # 좌표 로그

        # 바운딩 박스 그리기
        cv2.rectangle(np_img, (x0, y0), (x1, y1), color, 2)

        # 텍스트
        text = f"{en_tag} {i}"
        font = cv2.FONT_HERSHEY_SIMPLEX
        font_scale = 0.3
        thickness = 1
        (w, h), _ = cv2.getTextSize(text, font, font_scale, thickness)
        margin = 5 # 박스와 텍스트 간격
        tx = x0 # 텍스트 x 좌표(왼쪽 정렬)
        ty = max(0, y0 - margin)
        cv2.putText(np_img, text, (tx + 2, ty - 2), font, font_scale, (255, 255, 255), thickness, cv2.LINE_AA) # 흰색 텍스트로 표시


# 시각화
plt.figure(figsize=(10, 10))
plt.imshow(cv2.cvtColor(np_img, cv2.COLOR_BGR2RGB))
plt.axis("off")
plt.show()
```

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
