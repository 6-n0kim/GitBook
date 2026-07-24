---
icon: table
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

# 정형 데이터 비식별 실습

## 정형 데이터란?

행(row)과 열(column)로 구성된 표 형태의 데이터로, 이름·주소·생년월일·계좌번호처럼 값의 의미와 형식이 컬럼 단위로 명확하게 정의되어 있는 데이터를 의미합니다. (예: CSV, 엑셀, DB 테이블)

## 비식별화란?

데이터에 포함된 개인정보의 일부 또는 전부를 삭제하거나 다른 값으로 대체하여, 특정 개인을 식별하거나 유추할 수 없도록 만드는 일련의 과정입니다.

정형 데이터는 비정형 데이터(텍스트/이미지/영상/음성)와 달리 **컬럼 구조가 고정**되어 있기 때문에, 비식별 처리에 앞서 **데이터 정제(Data Cleansing)** 를 통해 결측치·중복·형식 불일치·이상치 등을 먼저 정리하는 과정이 필수적입니다.

**실습 내용:**

* 데이터 정제 - Data Cleansing
* 개인정보 비식별처리 (가명처리 / 익명처리)
* 데이터 시각화 및 검증
* 합성 데이터 생성

**대상 데이터:**

* CSV 파일
* Excel 파일
* 데이터베이스 덤프 파일
* JSON 형식의 구조화된 데이터
