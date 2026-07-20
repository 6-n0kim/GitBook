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

# masking(마스크) 처리

### **5. 마스킹 기능 및 적용**

* 탐지된 얼굴 영역을 **검은색 사각형**으로 완전히 덮어 가리는 방식입니다.
* 이 방식은 가장 강력한 비식별화 처리 중 하나로, 얼굴 정보를 **완전히 차단**합니다.

```python
masked_frames = [] # 마스킹된 프레임들을 담는 리스트

for idx in range(len(original_frames)):
  frame = original_frames[idx]
  image_pil = Image.fromarray(cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)).copy()
  draw = ImageDraw.Draw(image_pil)

  # 얼굴 박스마다 검은색 사각형으로 덮기
  for (x0, y0, x1, y1) in face_box_list[idx]:
    draw.rectangle([x0, y0, x1, y1], fill='black')

  # 번호판 마스킹 (검은색 사각형)
  for (x0, y0, x1, y1) in plate_box_list[idx]:
    draw.rectangle([x0, y0, x1, y1], fill='black')

  masked_frames.append(np.array(image_pil))

def show_blurred(idx):
    plt.figure(figsize=(8, 6))
    plt.imshow(masked_frames[idx])
    plt.axis("off")
    plt.show()

slider = widgets.IntSlider(value=0, min=0, max=len(masked_frames)-1, step=1, description='Frame:')
widgets.interact(show_blurred, idx=slider)
```
