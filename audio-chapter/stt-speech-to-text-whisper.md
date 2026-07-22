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



### **2. STT (Whisper)**

* Whisper 모델을 이용해 음성을 텍스트로 변환하고, 각 구간(segment)의 시작·끝 시간(timestamp)도 함께 얻습니다.
* 글자 단위가 아니라 구간별로 타임스탬프를 알아야, 개인정보가 포함된 구간을 정확히 탐지해 삐 처리(beep)할 수 있습니다.

```python
!pip install -q openai-whisper # STT 라이브러리 다운로드
!pip install -q transformers==4.44.2 pydub==0.25.1 soundfile==0.12.1 pandas==2.2.2
!apt-get install -y ffmpeg -qq

print("✅ 모든 라이브러리 및 ffmpeg 시스템 패키지 설치 완료!")
```

```python
import whisper
import pandas as pd
from IPython.display import display

# 1. Whisper base 모델 로드
model = whisper.load_model("base")

# 2. 음성 ➔ 텍스트 변환 및 단어 단위 타임스탬프 추출
result = model.transcribe(audio_path, word_timestamps=True)
full_text = result["text"]

print("▼ 전체 STT 변환 텍스트:")
print(full_text)
print("\n" + "="*50 + "\n")

# 3. 단어별 타임스탬프 데이터프레임 구성
timestamp = result["segments"] # 구간(문장/절) 리스트
word_rows = []
for seg in timestamp:
    for word in seg.get("words", []):
        word_rows.append({
            "단어": word["word"].strip(),
            "시작(초)": round(float(word["start"]), 2),
            "끝(초)": round(float(word["end"]), 2),
            "길이(초)": round(float(word["end"] - word["start"]), 2)
        })

df_words = pd.DataFrame(word_rows)
print("▼ 단어별 타임스탬프 표:")
display(df_words)
```

<figure><img src="../../.gitbook/assets/오디오STT.png" alt="" width="368"><figcaption></figcaption></figure>
