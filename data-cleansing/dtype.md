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

이 데이터는 `생년월일` 컬럼의 형식이 일부 섞여 있습니다. (`1992-11-07` vs `19890115`)

{% hint style="warning" %}
`생년월일`에는 앞서 ①에서 남겨둔 결측치(NaN)가 그대로 있습니다. 형식 검사 시 NaN을 문자열 `'nan'`으로 잘못 처리하지 않도록 `dropna()`나 `na=False` 옵션으로 결측치를 먼저 제외하고 확인합니다.
{% endhint %}

```python
# 결측이 아닌 값만 대상으로 형식(길이) 분포 확인
print(clean_df['생년월일'].dropna().astype(str).str.len().value_counts())

# 하이픈(-)이 없는 값 찾아보기 (NaN은 자동으로 False 처리)
mask = ~clean_df['생년월일'].astype(str).str.contains('-', na=False)
print(clean_df.loc[mask & clean_df['생년월일'].notnull(), '생년월일'])
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

```python
# 형식을 'YYYY-MM-DD'로 통일 (NaN은 그대로 유지)
def normalize_date(x):
    if pd.isnull(x):
        return x
    x = str(x)
    if '-' not in x and len(x) == 8:
        return f"{x[:4]}-{x[4:6]}-{x[6:]}"
    return x

clean_df['생년월일'] = clean_df['생년월일'].apply(normalize_date)
```

`주소` 컬럼도 일부는 시/도명만 있고 상세주소가 없는 경우가 있습니다. 비식별 처리(예: 주소 일부 마스킹) 전에 이런 불완전한 값을 확인해두는 것이 좋습니다.

```python
# 주소 길이가 유난히 짧은(상세주소 누락 의심) 행 확인 (결측치 제외)
short_addr = clean_df[clean_df['주소'].notnull() & (clean_df['주소'].str.len() < 10)]
print(short_addr[['record_id', '주소']])
```

\[출력 결과]

```
  record_id     주소
6    R00005  경기도
```
