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

# STT(Speech-to-Text) - Whisper

```python
!pip uninstall -y torch torchvision torchaudio
!pip cache purge
!pip install torch==2.6.0 torchvision==0.21.0 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu118
```

### **2. STT (Whisper)**

* Whisper 모델을 이용해 음성을 텍스트로 변환하고, 각 구간(segment)의 시작·끝 시간(timestamp)도 함께 얻습니다.
* 글자 단위가 아니라 구간별로 타임스탬프를 알아야, 개인정보가 포함된 구간을 정확히 탐지해 삐 처리(beep)할 수 있습니다.

```python
!pip install -q openai-whisper # STT 라이브러리 다운로드
```

```python
import whisper
import pandas as pd
from IPython.display import display

# 모델 로드 (base, small, medium 중 선택)
model = whisper.load_model("base")

# 음성 -> 텍스트 + 세그먼트(timestamp) 추출(각 단어별로 word, start, end, duration)
result = model.transcribe(audio_path, word_timestamps=True)
full_text = result["text"] # 전체 텍스트
print("전체 텍스트:\n", full_text)

timestamp = result["segments"] # 구간(문장/절) 리스트
word_rows = [] # 단어별(Word-level) 정보를 담을 리스트
for seg in timestamp:
    for word in seg.get("words", []):
        word_rows.append({
            "단어": word["word"].strip(), # 단어 문자열 (양쪽 공백 제거)
            "시작(초)": round(float(word["start"]), 2),
            "끝(초)": round(float(word["end"]), 2),
            "길이(초)": round(float(word["end"] - word["start"]), 2) # 단어 길이(초)
        })

df = pd.DataFrame(word_rows)
print("단어별 타임스탬프:\n")
display(df)
```
