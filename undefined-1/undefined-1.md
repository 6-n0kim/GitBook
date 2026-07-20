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

# 비디오 모든 프레임 확인

### 2. **모든 프레임 확인**

* 영상이 잘 업로드 되었는지 확인하기 위해 모든 프레임을 확인합니다.
* 위 슬라이더 UI를 이동하여 확인 가능.

```python
import numpy as np
from IPython.display import display
import ipywidgets as widgets

cap = cv2.VideoCapture(video_path) # 비디오 파일 열기(핸들 생성)
frames = [] # 모든 프레임을 담아둘 리스트

while True:
    ret, frame = cap.read() # 한 프레임 읽기(ret: 성공 여부, frame: BGR 이미지)
    if not ret or frame is None:
        break
    frame_rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB) # OpenCV BGR → 시각화용 RGB 변환
    frames.append(frame_rgb)

cap.release() # 비디오 핸들 해제(리소스 반환)
print(f"총 {len(frames)}개의 프레임이 로드되었습니다.")

# 프레임 보기 함수
def show_frame(idx):
  plt.figure(figsize=(8, 6))
  plt.imshow(frames[idx])
  plt.axis("off")
  plt.show()

# 슬라이더 UI 만들기
slider = widgets.IntSlider(value=0, min=0, max=len(frames)-1, step=1, description='Frame:')
widgets.interact(show_frame, idx=slider)
```

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="563"><figcaption></figcaption></figure>
