---
icon: headphones
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

# Colab 음성 파일 업로드

## **1. 오디오 업로드**
- 1. Colab 좌측 사이드바의 📁 [파일] 아이콘을 클릭합니다.
- 2. 구글 드라이브에서 로컬에 받은 'audio_01.wav' 파일(또는 02, 03)을 좌측 파일 탐색기 창으로 [드래그 앤 드롭] 해주세요.

<figure><img src="../../.gitbook/assets/오디오업로드.png" alt=""><figcaption></figcaption></figure>

```python
import os
from IPython.display import display, Audio

# 실습 기본 대상 파일명 (audio_01.wav ~ 03.wav 중 선택)
target_file = 'audio_01.wav'

if not os.path.exists(target_file):
  print(f"❌ '{target_file}' 파일이 현재 Colab 환경에 없습니다. 업로드 가이드를 확인 후 다시 시도해 주세요.")
else:
  audio_path = target_file
  print(f"'{audio_path}' 오디오 로드 완료!")
  display(Audio(audio_path))
```


