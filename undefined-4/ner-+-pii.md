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

### **2. NER(Named Entity Recognition) + 정규식(Regex)으로 개인정보 검출**

* 1\) 한국어 특화 KoELECTRA KLUE‑NER 모델로 텍스트 내 사람(PER), 기관(ORG), 지명(LOC), 날짜(DATE) 같은 의미 있는 객체를 찾습니다.\
  2\) 정규식은 전화번호·이메일·주민등록번호처럼 형식이 명확한 PII를 정확하게 추가 검출합니다.
* 사람 "이름”== NER, “010-1234-5678” == regex가 더 정확하므로 두 방법을 병행하면 서로 보완하여 탐지 성능이 크게 향상됩니다.

###

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

# NER 실행
ner_spans = [] # PII 담을 빈 리스트
for ent in ner_pipe(text):
    ner_spans.append((ent["entity_group"], ent["start"], ent["end"], ent["word"]))

print("NER로 찾은 개인정보:")
for i,(label,start,end,val) in enumerate(ner_spans):
    print(f"{i}. [{label}] “{val}” ({start}-{end})")
```

```python
import re

# 정규식 패턴 정의: (패턴, 라벨)
patterns = [
    (r"\b01[016789]-\d{3,4}-\d{4}\b", "PHONE"),
    (r"[\w\.-]+@[\w\.-]+\.\w+\b", "EMAIL"),
    (r"\b\d{6}-\d{7}\b", "RRN"),
]

# 정규식으로 탐지된 결과 리스트
regex_spans = []

for pattern, label in patterns:
    for match in re.finditer(pattern, text):
        regex_spans.append((
            label,             # 라벨 (PHONE, EMAIL, RRN)
            match.start(),     # 시작 위치
            match.end(),       # 끝 위치
            match.group()      # 탐지된 문자열
        ))

print("정규식으로 찾은 개인정보:")
for i,(label,start,end,val) in enumerate(regex_spans):
    print(f"{i}. [{label}] “{val}” ({start}-{end})")
```

```python
# 두 결과를 하나로 합치기
all_spans = ner_spans + regex_spans

# 중복 제거
seen = set()
unique_spans = [] # 통합 결과 리스트

for label, start, end, val in all_spans:
    if (start, end) not in seen:
        seen.add((start, end))
        unique_spans.append((label, start, end, val))

# 라벨 기준 정렬
unique_spans.sort(key=lambda x: x[0])

print("통합된 PII (NER+regex):")
for i, (label,start,end,val) in enumerate(unique_spans):
    print(f"{i}. [{label}] “{val}” ({start}-{end})")
```
