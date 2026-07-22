---
icon: shield-user
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

# 비식별화 기법

정형 데이터에 적용할 수 있는 다양한 비식별화 기법입니다.

## 1. 마스킹 (Masking)

개인정보를 특정 문자로 대체하는 방법입니다.

```python
# 전화번호 마스킹
def mask_phone(phone):
    if pd.isna(phone):
        return phone
    return phone[:3] + '-****-' + phone[-4:]

df['전화번호_마스킹'] = df['전화번호'].apply(mask_phone)

# 이메일 마스킹
def mask_email(email):
    if pd.isna(email):
        return email
    username, domain = email.split('@')
    return username[:2] + '***@' + domain

df['이메일_마스킹'] = df['이메일'].apply(mask_email)
```

## 2. 삭제 (Deletion)

개인정보 컬럼을 완전히 제거하는 방법입니다.

```python
# 민감 컬럼 제거
sensitive_columns = ['주민등록번호', '여권번호', '운전면허번호']
df_anonymized = df.drop(columns=sensitive_columns)
```

## 3. 범주화 (Generalization)

세부 정보를 범주화하는 방법입니다.

```python
# 나이 범주화
def categorize_age(age):
    if age < 20:
        return '10대'
    elif age < 30:
        return '20대'
    elif age < 40:
        return '30대'
    else:
        return '40대 이상'

df['나이_범주'] = df['나이'].apply(categorize_age)

# 주소 범주화 (시/도 수준)
df['주소_범주'] = df['주소'].apply(lambda x: x.split()[0] if pd.notna(x) else x)
```

## 4. 치환 (Substitution)

실제 값을 가상의 값으로 대체하는 방법입니다.

```python
import hashlib

# 이름 해시 치환
def hash_name(name):
    if pd.isna(name):
        return name
    return hashlib.sha256(name.encode()).hexdigest()[:8]

df['이름_해시'] = df['이름'].apply(hash_name)
```

## 5. 데이터 교환 (Data Swapping)

데이터 행 간 값을 교환하는 방법입니다.

```python
import numpy as np

# 특정 컬럼 값 교환
def swap_values(series, swap_ratio=0.1):
    n = int(len(series) * swap_ratio)
    swap_idx = np.random.choice(series.index, n, replace=False)
    series_copy = series.copy()
    series_copy[swap_idx] = series.sample(n).values
    return series_copy

df['전화번호_교환'] = swap_values(df['전화번호'])
```

## 다음 단계

비식별화가 완료되면 [결과 검증](validation.md) 단계로 진행합니다.
