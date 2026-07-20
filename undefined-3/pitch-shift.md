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

# Pitch-Shift 변조

### **5. 음성 변조 (Pitch Shift)**

* 삐 처리된 오디오를 옥타브 단위로 높이/낮춰서 음성 톤을 변조합니다.
* 원음만 노출될 경우 성별·개인 식별 위험이 남을 수 있어, 톤 변조로 추가 익명성을 보강합니다.
* 음성의 속도 변화를 원본과 동일하게 주고 싶을 때, librosa로 피치 쉬프트만 수행하면 됩니다.(ex. librosa.effects.pitch\_shift)
* 속도 느려짐 현상은 pydub.effects.speedup(내부적으로 ffmpeg atempo)로 속도만 역보정하면 됩니다.(제외)

```python
from io import BytesIO
from IPython.display import Audio, display
from pydub import AudioSegment

# Pitch shift 함수: 옥타브 단위 (-1.0 낮추기, +1.0 높이기)
def pitch_shift(sound: AudioSegment, octaves: float) -> AudioSegment:
    new_rate = int(sound.frame_rate * (2.0 ** octaves))
    shifted = sound._spawn(sound.raw_data, overrides={'frame_rate': new_rate})

    return shifted.set_frame_rate(sound.frame_rate)

# 변조 적용
print("Pitch-shifting 음성 변조 중...")
# output: 앞서 삐 처리된 AudioSegment / 높은 음으로 변경 시 octaves를 양수로 변경
transformed = pitch_shift(output, octaves=-0.5)

# 메모리상 WAV 컨테이너로 export
buf2 = BytesIO()
transformed.export(buf2, format="wav")
buf2.seek(0)

# 변조된 오디오 재생
print("변조된 음성 재생:")
display(Audio(data=buf2.read(), rate=transformed.frame_rate))
```
