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

# 수동 변환 기능

### **6. 수동 변환 기능 및 적용**

* 탐지된 PII를 **직접 하나씩 확인**하고, 원하는 문자열을 **수동**으로 바꿉니다.
* 단순 삭제/마스킹 외에 **맞춤형** 비식별 처리가 필요할 때 유용합니다.

```python
# 탐지된 PII 목록 출력 및 수정 입력 받기
print("탐지된 개인정보 목록 (수정할 부분만 엔터 후 텍스트 입력):")
replacements = {} # 수정된 문자열 리스트
for i, (label, start, end, val) in enumerate(unique_spans):
    new = input(f"{i}. [{label}] “{val}” -> ")
    if new.strip(): # 입력한 값이 있으면 저장
        replacements[val] = new.strip()

# 원문에 치환 적용
result = text
for label, start, end, orig in sorted(unique_spans, key=lambda x: x[1], reverse=True):
    if orig in replacements:
        new = replacements[orig]
        # orig 가 들어 있는 위치(start:end)를 슬라이싱으로 대체
        result = result[:start] + new + result[end:]

print("\n수정된 전체 텍스트:")
print(result)
```

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
