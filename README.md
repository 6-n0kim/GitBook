---
icon: file-video
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

# 비정형 데이터 비식별 실습

영상, 이미지, 음성, 텍스트 등 비구조화된 데이터에서 개인정보를 비식별화하는 실습입니다.

**실습 내용:**
- 영상 비식별 (얼굴/번호판 탐지, 모자이크, 마스킹)
- 이미지 비식별 (얼굴/번호판 탐지, 바운딩 박스, 모자이크)
- 음성 비식별 (STT, 음성 변조, TTS)
- 텍스트 비식별 (NER, 정규식 PII 검출, 마스킹)

**대상 데이터:**
- 영상 파일 (MP4, AVI 등)
- 이미지 파일 (JPG, PNG 등)
- 음성 파일 (WAV, MP3 등)
- 비구조화 텍스트 문서 (TXT 등)

**공통 사전 준비:**
- Google Colaboratory 설치 및 설정

## Google Colaboratory 설치 (공통)

1. 구글 로그인
2. 구글 드라이브 접속
3. 내 드라이브에서 우클릭
4. 연결 더보기

<figure><img src=".gitbook/assets/화면 캡처 2025-08-09 145855.png" alt="" width="254"><figcaption></figcaption></figure>

5. \[Colaboratory] 검색 후 설치

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

6. \[Colaboratory] 생성

<figure><img src=".gitbook/assets/image (24).png" alt="" width="235"><figcaption></figcaption></figure>
