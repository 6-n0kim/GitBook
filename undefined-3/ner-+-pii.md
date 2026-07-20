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

# NER + 정규식 PII 검출

### 3. **NER(Named Entity Recognition) + 정규식(Regex)으로 개인정보 검출**

* 1\) 한국어 특화 KoELECTRA KLUE‑NER 모델로 텍스트 내 사람(PER), 기관(ORG), 지명(LOC), 날짜(DATE) 같은 의미 있는 객체를 찾습니다.\
  2\) 정규식은 전화번호·이메일·주민등록번호처럼 형식이 명확한 PII를 정확하게 추가 검출합니다.
* 사람 "이름”== NER, “010-1234-5678” == regex가 더 정확하므로 두 방법을 병행하면 서로 보완하여 탐지 성능이 크게 향상됩니다.

```python
from transformers import AutoTokenizer, AutoModelForTokenClassification, pipeline

# NER 모델 로드(한국어 특화)
model_name = "soddokayo/koelectra-base-klue-ner"
# 토크나이저와 모델 불러오기
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForTokenClassification.from_pretrained(model_name)

# 파이프라인 구성
ner_pipe = pipeline(
    task="ner",             # 토큰 단위로 엔티티 인식
    model=model,
    tokenizer=tokenizer,
    grouped_entities=True   # "홍 길 동" → "홍길동"
)

# STT 방법으로 얻은 텍스트 입력
ner_spans = ner_pipe(full_text)

# 결과 출력
for ent in ner_spans:
    print(f"{ent['entity_group']:6s} {ent['start']:4d}-{ent['end']:4d}  “{ent['word']}”")
```

<figure><img src="../.gitbook/assets/image (25).png" alt="" width="292"><figcaption></figcaption></figure>

```python
import re

# 정규식 패턴 정의: (패턴, 라벨)
patterns = [
    (r"\b01[016789]-\d{3,4}-\d{4}\b", "PHONE"),
    (r"\b[\w\.-]+@[\w\.-]+\.\w+\b", "EMAIL"),
    (r"\b\d{6}-\d{7}\b", "RRN"),
]

# 정규식으로 탐지된 결과 리스트
regex_spans = []

for pattern, label in patterns:
    for match in re.finditer(pattern, full_text):
        regex_spans.append((
            match.group(),     # 탐지된 문자열
            label,             # 라벨 (PHONE, EMAIL, RRN)
            match.start(),     # 시작 위치
            match.end()        # 끝 위치
        ))

# 결과 출력
print("정규식으로 찾은 개인정보:")
for val, label, start, end in regex_spans:
    print(f"{label:6s} {start:4d}-{end:4d}  “{val}”")
```

```python
# 두 결과를 하나로 합치기
pii_spans = ner_spans + regex_spans

# 중복 제거
unique_pii_spans = []
seen = set()

for span in pii_spans:
    # 딕셔너리 기반 처리
    if isinstance(span, dict):
        key = (span["start"], span["end"])
        val = span["word"]
        label = span.get("entity_group", "UNK")
    else:
        # 튜플 기반 처리
        val, label, start, end = span
        key = (start, end)

    if key not in seen:
        seen.add(key)
        if isinstance(span, dict):
            unique_pii_spans.append((val, label, key[0], key[1]))
        else:
            unique_pii_spans.append(span)

# 결과 출력
print("NER + 정규식으로 합쳐진 개인정보 탐지 결과:")
for val, label, start, end in unique_pii_spans:
    print(f"{label:6s} {start:4d}-{end:4d}  “{val}”")
```

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
