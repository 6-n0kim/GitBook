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

# PII 삭제 기능

### **3. 삭제 기능(Delete)**

* 탐지된 PII(개인정보) 구간을 아예 잘라내서 제거합니다.
* 민감 정보가 원문에 전혀 남지 않도록 할 때 사용합니다.

```python
# 삭제 함수
def anonymize_delete(text, spans):
    # start 값이 큰 순서로 정렬 → 뒤에서부터 잘라야 인덱스 오류 방지
    for label, start, end, val in sorted(spans, key=lambda x: x[1], reverse=True):
        # text = text[:start] + text[end:] # 완전 삭제
        text = text[:start] + "[]" + text[end:] # []로 대체 (삭제 위치 표시)
    return text

# 적용
text_deleted = anonymize_delete(text, unique_spans)
print("삭제 결과:\n", text_deleted)
```
