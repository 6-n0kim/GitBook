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

# 데이터 형식 통일

### 5. 데이터 형식 통일

**5-1 생년월일**

이 데이터는 `생년월일` 컬럼의 형식이 일부 섞여 있습니다. (`1992-11-07` vs `19890115`)

```python
# 1. 결측치 제외 및 문자열 변환
s_birth = clean_df['생년월일'].dropna().astype(str)

# 2. 글자 수(길이)별로 그룹화하여 출력
for length, group in s_birth.groupby(s_birth.str.len()):
    print(f"{length}자리 문자열 개수")
    print(f"{len(group)}개")
    print(group.unique())
    print()
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

```python
# 형식을 'YYYY-MM-DD'로 통일
def normalize_date(x):
    if pd.isnull(x):
        return x
    x = str(x)
    if '-' not in x and len(x) == 8:
        return f"{x[:4]}-{x[4:6]}-{x[6:]}"
    return x

clean_df['생년월일'] = clean_df['생년월일'].apply(normalize_date)
```

**5-2 주소**

`주소` 컬럼도 일부는 시/도명만 있고 상세주소가 없는 경우가 있습니다. 비식별 처리(예: 주소 일부 마스킹) 전에 이런 불완전한 값을 확인해두는 것이 좋습니다.

```python
# 주소 길이가 유난히 짧은(상세주소 누락 의심) 행 확인
short_addr = clean_df[clean_df['주소'].notnull() & (clean_df['주소'].str.len() < 10)]
display(short_addr[['record_id', '주소']])
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>
