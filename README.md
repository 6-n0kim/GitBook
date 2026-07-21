---
icon: database
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

# 데이터 비식별 실습

데이터 비식별화 실습을 위한 가이드입니다.

## 실습 유형 선택

[정형 데이터 비식별 실습](structured/README.md)

[비정형 데이터 비식별 실습](unstructured/README.md)

---

## 정형 데이터 비식별 실습

CSV, Excel 등 구조화된 데이터에서 개인정보를 비식별화하는 실습입니다.

**실습 내용:**
- CSV/Excel 파일 업로드 및 로드
- 데이터 전처리 및 탐색
- 다양한 비식별화 기법 적용 (마스킹, 삭제, 범주화, 치환)
- 결과 검증 및 내보내기

**대상 데이터:**
- CSV 파일
- Excel 파일
- 데이터베이스 덤프 파일
- JSON 형식의 구조화된 데이터

[정형 데이터 비식별 실습 시작하기 →](structured/README.md)

---

## 비정형 데이터 비식별 실습

영상, 이미지, 음성, 텍스트 등 비구조화된 데이터에서 개인정보를 비식별화하는 실습입니다.

**실습 유형:**
- **영상 비식별**: 얼굴/번호판 탐지, 모자이크, 마스킹
- **이미지 비식별**: 얼굴/번호판 탐지, 바운딩 박스, 모자이크
- **음성 비식별**: STT, 음성 변조, TTS
- **텍스트 비식별**: NER, 정규식 PII 검출, 마스킹

**공통 사전 준비:**
- Google Colaboratory 설치 및 설정
- Google 드라이브 연결

[비정형 데이터 비식별 실습 시작하기 →](unstructured/README.md)

---

## Google Colaboratory 설치 (공통)

비정형 데이터 비식별 실습을 위한 Google Colaboratory 설치 방법입니다.

1. 구글 로그인
2. 구글 드라이브 접속
3. 내 드라이브에서 우클릭
4. 연결 더보기

<figure><img src=".gitbook/assets/화면 캡처 2025-08-09 145855.png" alt="" width="254"><figcaption></figcaption></figure>

5. \[Colaboratory] 검색 후 설치

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

6. \[Colaboratory] 생성

<figure><img src=".gitbook/assets/image (24).png" alt="" width="235"><figcaption></figcaption></figure>
