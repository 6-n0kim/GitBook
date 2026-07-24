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

# blur(블러) 처리

### **4. 블러 기능 및 적용**

* 탐지된 영역의 픽셀 값들을 평균화하여 경계와 상세 형태를 부드럽게 뭉개는 방식입니다.
* 이는 얼굴의 시각적 식별을 방지하기 위한 **비식별화 처리 방법 중 하나**입니다.

```python
# 원본 다시 로드
img_blur = Image.open(img_path).convert("RGB")

# 얼굴 영역 블러
for box in face_boxes:
    x0, y0, x1, y1 = map(int, box) # 좌표를 정수형으로 변환 (픽셀 인덱스)
    region = img_blur.crop((x0, y0, x1, y1)) # 해당 박스 영역만 잘라냄 (PIL Crop: [x0,y0,x1,y1))
    region = region.filter(ImageFilter.GaussianBlur(radius=15)) # radius: 블러 강도 (값이 클수록 더 흐릿해짐)
    img_blur.paste(region, (x0, y0)) # 블러 처리한 영역을 원본 위치에 다시 붙여넣기

# 5) 번호판 영역 블러
for box in plate_boxes:
    x0, y0, x1, y1 = map(int, box)
    region = img_blur.crop((x0, y0, x1, y1))
    region = region.filter(ImageFilter.GaussianBlur(radius=15))
    img_blur.paste(region, (x0, y0))

plt.figure(figsize=(6,6))
plt.imshow(img_blur)
plt.axis("off")
plt.show()
```

<figure><img src="../.gitbook/assets/image (7).png" alt="" width="537"><figcaption></figcaption></figure>
