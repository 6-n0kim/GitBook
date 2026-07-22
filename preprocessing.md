---
icon: wrench
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

# 데이터 전처리

비식별화 전 데이터 전처리 단계입니다.

## 1. 데이터 탐색

```python
# 컬럼명 확인
print(df.columns.tolist())

# 데이터 타입 확인
print(df.dtypes)

# 결측치 확인
print(df.isnull().sum())
```

## 2. 불필요한 컬럼 제거

```python
# 특정 컬럼 제거
df_clean = df.drop(columns=['불필요한_컬럼1', '불필요한_컬럼2'])

# 결측치가 많은 컬럼 제거
threshold = len(df) * 0.5
df_clean = df.dropna(axis=1, thresh=threshold)
```

## 3. 데이터 타입 변환

```python
# 날짜 형식 변환
df['날짜_컬럼'] = pd.to_datetime(df['날짜_컬럼'])

# 숫자형 변환
df['숫자_컬럼'] = pd.to_numeric(df['숫자_컬럼'], errors='coerce')
```

## 4. 중복 데이터 제거

```python
# 중복 행 제거
df_unique = df.drop_duplicates()

# 특정 컬럼 기준 중복 제거
df_unique = df.drop_duplicates(subset=['이름', '전화번호'])
```

## 다음 단계

전처리가 완료되면 [비식별화](anonymization.md) 단계로 진행합니다.
