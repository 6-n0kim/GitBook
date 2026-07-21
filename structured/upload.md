---
icon: upload
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

# CSV/Excel 파일 업로드

Google Colaboratory에 정형 데이터 파일을 업로드하는 방법입니다.

## 1. Google Colaboratory 접속

1. [Google Colaboratory](https://colab.research.google.com/) 접속
2. 새 노트북 생성

## 2. Google 드라이브 마운트

```python
from google.colab import drive
drive.mount('/content/drive')
```

## 3. CSV 파일 로드

```python
import pandas as pd

# 방법 1: 파일 업로드
from google.colab import files
uploaded = files.upload()

# 방법 2: Google 드라이브에서 로드
df = pd.read_csv('/content/drive/MyDrive/your_data.csv')
```

## 4. Excel 파일 로드

```python
# Excel 파일 로드
df_excel = pd.read_excel('/content/drive/MyDrive/your_data.xlsx')
```

## 5. 데이터 확인

```python
# 상위 5개 행 확인
print(df.head())

# 데이터 정보 확인
print(df.info())

# 통계 정보 확인
print(df.describe())
```

## 다음 단계

파일 업로드가 완료되면 [데이터 전처리](preprocessing.md) 단계로 진행합니다.
