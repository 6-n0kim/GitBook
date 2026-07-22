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

# blur(모자이크) 처리

### **4. 블러 기능 및 적용**

* 해당 영역을 **Gaussian Blur**를 사용해 흐리게 만듭니다.
* 이는 얼굴의 시각적 식별을 방지하기 위한 **비식별화 처리 방법 중 하나**입니다.

```python
blurred_frames = [] # 블러 처리된 프레임들을 담을 리스트

for idx in range(len(original_frames)):
  frame = original_frames[idx] # 원본 BGR 프레임 가져오기
  face_boxes = face_box_list[idx] # 해당 프레임의 얼굴 바운딩 박스 목록
  plate_boxes = plate_box_list[idx] # 해당 프레임의 번호판 바운딩 박스 목록

  image_pil = Image.fromarray(cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)).copy()

  # 얼굴 블러 처리
  for (x0, y0, x1, y1) in face_boxes:
    region = image_pil.crop((x0, y0, x1, y1)) # 해당 좌표 영역만 잘라내기
    region = region.filter(ImageFilter.GaussianBlur(radius=15)) # 블러 강도(값이 클수록 흐림)
    image_pil.paste(region, (x0, y0)) # 블러된 영역을 원래 위치에 덮어쓰기

  # 번호판 블러 처리
  for (x0, y0, x1, y1) in plate_boxes:
    region = image_pil.crop((x0, y0, x1, y1))
    region = region.filter(ImageFilter.GaussianBlur(radius=15))
    image_pil.paste(region, (x0, y0))

  # 최종 RGB 변환 후 저장
  blurred_frames.append(np.array(image_pil)) # PIL 이미지를 RGB numpy 배열로 변환해 리스트에 추가

def show_blurred(idx):
    plt.figure(figsize=(8, 6))
    plt.imshow(blurred_frames[idx])
    plt.axis("off")
    plt.show()

slider = widgets.IntSlider(value=0, min=0, max=len(blurred_frames)-1, step=1, description='Frame:')
widgets.interact(show_blurred, idx=slider)
```

<figure><img src="../../.gitbook/assets/image (3).png" alt="" width="563"><figcaption></figcaption></figure>
