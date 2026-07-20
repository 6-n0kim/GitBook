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

* PII가 포함된 구간에 맞게 ‘삐\~’소리로 대체합니다.
* 개인정보를 음성에서 완전히 숨겨, 남용·유출 위험을 제거하기 위함입니다.

```python
!pip install -q pydub
```

```python
from io import BytesIO
from IPython.display import Audio, display
from pydub import AudioSegment
from pydub.generators import Sine
import math

# 원본 오디오 로드
original_audio = AudioSegment.from_file(audio_path) # mp3/wav 등 지원

# 개인정보가 포함된 단어들의 정확한 start/end(ms) 구간 수집
beep_ranges_ms = []

# 세그먼트 단위로 돌며 PII 포함 구간만 삐로 교체
for seg in timestamp:
    # 각 segment 안의 단어별 타임스탬프
    for word in seg.get("words", []):
        word_text = word["word"].strip()
        word_start_ms = int(float(word["start"]) * 1000)
        word_end_ms   = int(float(word["end"]) * 1000)

        # PII 리스트에서 동일 단어나 포함관계가 있으면 삐 처리 구간으로 추가
        for pii_text, _, _, _ in unique_pii_spans:
            if pii_text.strip() in word_text or word_text in pii_text.strip():
                beep_ranges_ms.append((word_start_ms, word_end_ms))

# 중복 제거(set) 후, 시작 시점 기준으로 오름차순 정렬
beep_ranges_ms = sorted(set(beep_ranges_ms), key=lambda x: x[0])

# 삐 처리된 최종 AudioSegment
output = AudioSegment.empty()
last_end = 0

for start_ms, end_ms in beep_ranges_ms:
    output += original_audio[last_end:start_ms] # PII 전까지 원본 이어붙이기
    duration = end_ms - start_ms
    # 구간 길이에 딱 맞는 삐 생성
    if duration > 0:
        output += (
            Sine(1000).to_audio_segment(duration=duration)
            .set_frame_rate(original_audio.frame_rate)
            .set_channels(original_audio.channels)
            .set_sample_width(original_audio.sample_width)
        )
    last_end = end_ms

# 마지막 PII 뒤 남은 원본 음성 붙이기
output += original_audio[last_end:]

# BytesIO 버퍼에 WAV 컨테이너로 export
buf = BytesIO()
output.export(buf, format="wav")

# 재생을 위해 포인터를 처음으로 되돌리고, full WAV 바이트를 Audio에 넘김
print("개인정보 구간을 Beep 처리한 결과:")
buf.seek(0)
display(Audio(data=buf.read(), rate=output.frame_rate))

# 비교용 원본 오디오도 동일 방식으로 재생
buf_orig = BytesIO()
original_audio.export(buf_orig, format="wav")
print("원본 비교:")
buf_orig.seek(0)
display(Audio(data=buf_orig.read(), rate=original_audio.frame_rate))
```
