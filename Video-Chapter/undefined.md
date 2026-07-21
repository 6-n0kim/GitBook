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

torch 관련 다운로드및 버전 일

```python
# !pip install numpy==1.23.5
!pip uninstall torch torchvision torchaudio -y
!pip install torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu118
```

라이브러리 다운로드 및 얼굴 탐지 모델(facenet-pytorch)

```python
!pip install -q --upgrade transformers pillow==11.0.0 matplotlib ultralytics facenet-pytorch==2.5.2
```

자동차 번호판 탐지 모델(YOLOv26)

```python
!wget https://huggingface.co/yasirfaizahmed/license-plate-object-detection/resolve/main/best.pt \
     -O license_plate_yolov26.pt
```
