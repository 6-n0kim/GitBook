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

# 필요한 라이브러리 다운로드

라이브러리 다운로드 및 얼굴 탐지 모델(facenet-pytorch)

```python
!pip install -q transformers pillow matplotlib ultralytics facenet-pytorch==2.5.2
```

자동차 번호판 탐지 모델(YOLOv8)

```python
!wget https://huggingface.co/yasirfaizahmed/license-plate-object-detection/resolve/main/best.pt \
     -O license_plate_yolov8.pt
```
