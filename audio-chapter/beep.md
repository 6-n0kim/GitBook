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

# Beep(삐) 처리

### **4. 삐(beep) 처리**

* PII가 포함된 구간에 맞게  ‘삐\~’소리로 대체합니다.
* 개인정보를 음성에서 완전히 숨겨, 남용·유출 위험을 제거하기 위함입니다.

```python
import re
from io import BytesIO
from pydub import AudioSegment
from pydub.generators import Sine
from IPython.display import display, Audio

# 1. PII 단어와 매칭되는 오디오 타임스탬프(ms 단위) 추출
mask_intervals = []

for pii in unique_pii_spans:
    pii_clean = re.sub(r'[^\w\s]', '', pii["word"]) # 특수문자 제거
    for idx, row in df_words.iterrows():
        word_clean = re.sub(r'[^\w\s]', '', row["단어"])
        if word_clean and (word_clean in pii_clean or pii_clean in word_clean):
            start_ms = int(row["시작(초)"] * 1000)
            end_ms = int(row["끝(초)"] * 1000)
            mask_intervals.append((start_ms, end_ms))

# 2. 오디오 로드 및 Beep 음 생성 함수
sound = AudioSegment.from_file(audio_path)

def generate_beep(duration_ms):
    return Sine(1000).to_audio_segment(duration=duration_ms).apply_gain(-3)

# 3. 구간별 Beep 마스킹 조립
processed_sound = AudioSegment.empty()
current_pos = 0

# 타임스탬프 순 정렬
sorted_intervals = sorted(mask_intervals, key=lambda x: x[0])

for start_ms, end_ms in sorted_intervals:
    if start_ms > current_pos:
        processed_sound += sound[current_pos:start_ms]

    duration = end_ms - start_ms
    if duration > 0:
        processed_sound += generate_beep(duration)
        current_pos = end_ms

if current_pos < len(sound):
    processed_sound += sound[current_pos:]

# 4. 결과 저장 및 재생
output_filename = f"masked_beep_{audio_path}"
processed_sound.export(output_filename, format="wav")

print(f"🔒 [비식별화 완료] 결과 파일: {output_filename}")
display(Audio(output_filename))

# 비교용 원본 오디오도 동일 방식으로 재생
buf_orig = BytesIO()
sound.export(buf_orig, format="wav")
print("원본 비교:")
buf_orig.seek(0)
display(Audio(data=buf_orig.read(), rate=sound.frame_rate))
```

<figure><img src="../../.gitbook/assets/오디오Beep.png" alt=""><figcaption></figcaption></figure>
