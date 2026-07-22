# Colab 정형데이터 파일 업로드

### 1. **비디오 업로드**

* 1\. Colab 좌측 사이드바의 📁 \[파일] 아이콘을 클릭합니다.
* 2\. 구글 드라이브에서 로컬에 받은 'video\_01.mp4' 파일(또는 02, 03)을 좌측 파일 탐색기 창으로 \[드래그 앤 드롭] 해주세요.

```python
import os
from PIL import Image, ImageDraw, ImageFont, ImageFilter
import cv2
import matplotlib.pyplot as plt

# 실습 기본 대상 파일명 (video_01.mp4 ~ 03.mp4 중 선택)
target_file = 'video_01.mp4'

if not os.path.exists(target_file):
  print(f"❌ '{target_file}' 파일이 현재 Colab 환경에 없습니다. 업로드 가이드를 확인 후 다시 시도해 주세요.")
else:
  video_path = target_file
  print(f"'{video_path}' 비디오 로드 완료!")

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
  plt.show()
```