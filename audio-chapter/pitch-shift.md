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
import io
import librosa
import numpy as np
from IPython.display import Audio
from pydub import AudioSegment


# Pitch shift 함수: 옥타브 단위 (-1.0 낮추기, +1.0 높이기)
# 속도(1.0배)는 그대로 유지하면서 피치만 변경하도록 내부 구현 수정
def pitch_shift(seg: AudioSegment, octaves: float) -> AudioSegment:
    # 1. AudioSegment를 numpy float 배열로 변환
    samples = np.array(seg.get_array_of_samples(), dtype=np.float32)

    # 다채널(Stereo) 대응
    if seg.channels > 1:
        samples = samples.reshape((-1, seg.channels)).T

    # 정규화 (-1.0 ~ 1.0 범위)
    max_val = float(1 << (8 * seg.sample_width - 1))
    samples /= max_val

    # 2. 피치 변경 (1 옥타브 = 12 반음(semitones))
    n_steps = octaves * 12.0
    shifted = librosa.effects.pitch_shift(
        y=samples, sr=seg.frame_rate, n_steps=n_steps
    )

    # 3. 원래 정수형(int16 등) 데이터로 재복원
    if seg.channels > 1:
        shifted = shifted.T.reshape(-1)

    shifted_int = np.clip(shifted * max_val, -max_val, max_val - 1).astype(
        np.int16 if seg.sample_width == 2 else np.int32
    )

    # 4. 기존 오디오의 속도/샘플레이트를 유지한 채 AudioSegment 생성
    return seg._spawn(shifted_int.tobytes())

# 변조 적용 (-0.5 옥타브 적용 시 속도는 1.0배 유지됨)
print("Pitch-shifting 음성 변조 중...")
transformed = pitch_shift(processed_sound, octaves=-0.5)

# 메모리상 WAV 컨테이너로 export
buf2 = io.BytesIO()
transformed.export(buf2, format="wav")
buf2.seek(0)

# 변조된 오디오 재생
print("변조된 음성 재생:")
display(Audio(data=buf2.read(), rate=transformed.frame_rate))
```

<figure><img src="../../.gitbook/assets/오디오변조.png" alt=""><figcaption></figcaption></figure>
