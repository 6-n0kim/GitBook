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

정형 데이터(Structured Data) 비식별화 실습을 위한 가이드입니다.

## 실습 개요

이 실습에서는 CSV, Excel 등 정형화된 데이터셋에서 개인정보를 비식별화하는 방법을 학습합니다.

## 실습 목표

1. 정형 데이터 파일 업로드 및 로드
2. 개인정보(PII) 식별
3. 다양한 비식별화 기법 적용
4. 결과 데이터 검증

## 사전 준비

- Google Colaboratory 환경
- Python 기초 지식
- pandas 라이브러리 이해

## 데이터 형식

정형 데이터 비식별화 대상:
- CSV 파일
- Excel 파일
- 데이터베이스 덤프 파일
- JSON 형식의 구조화된 데이터
