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

# PII 마스킹 기능

### **4. 마스킹 기능 및 적용**

* 원본 길이를 유지하면서 개인정보 내용을 \*로 덮어씌웁니다.
* 길이만큼 덮어쓰는것도 단어의 수가 노출된다면 갯수를 정해주면 됩니다.

```python
# 마스킹 함수
def anonymize_mask(text, spans):
    for label, start, end, val in sorted(spans, key=lambda x: x[1], reverse=True):
        mask = "*" * (end - start) # 원래 길이만큼 마스킹
        text = text[:start] + mask + text[end:]
    return text

# 적용
text_masked = anonymize_mask(text, unique_spans)
print("마스킹 결과:\n", text_masked)
```

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
