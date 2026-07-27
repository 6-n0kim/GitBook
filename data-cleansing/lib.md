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

# 필요한 라이브러리 다운로드

### **1. 라이브러리 다운로드**

CSV 파일 입출력 및 수치 연산을 수행할 핵심 라이브러리(Pandas 2.2.2 / NumPy 2.0.2) 설치

```python
!pip install pandas==2.2.2 numpy==2.0.2
```

***

그래프 출력 시 한글이 깨지지 않도록 폰트 설치

```python
!apt-get -qq install fonts-nanum > /dev/null 2>&1
```
