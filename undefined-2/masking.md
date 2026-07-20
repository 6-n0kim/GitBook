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

* 탐지된 ㄱ 영역을 **검은색 사각형**으로 완전히 덮어 가리는 방식입니다.
* 이 방식은 가장 강력한 비식별화 처리 중 하나로, 얼굴 정보를 **완전히 차단**합니다.

```python
# 원본 다시 로드
img_mask = Image.open(img_path).convert("RGB")
draw_mask = ImageDraw.Draw(img_mask) # 그리기 도구(펜) 만들기

# 얼굴 마스킹
for box in face_boxes:
    x0, y0, x1, y1 = map(int, box)
    draw_mask.rectangle([x0, y0, x1, y1], fill="black")

# 번호판 마스킹
for box in plate_boxes:
    x0, y0, x1, y1 = map(int, box)
    draw_mask.rectangle([x0, y0, x1, y1], fill="black")

plt.figure(figsize=(6,6))
plt.imshow(img_mask)
plt.axis("off")
plt.show()
```
