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

# 범주화

### 5. 범주화 처리&#x20;

구체적인 수치나 단일값을 일정한 범위나 구간으로 묶어서 대표값으로 표현합니다.

* 1- `금액`과 `거래후잔액`은 정확한 금액 대신 십만 단위로 범주화

```python
# 1. 구간 문자열 생성 함수 정의
def make_range_str(series, decimals=100_000):
    lower = (np.floor(series / decimals) * decimals).astype(int)
    upper = (np.ceil(series / decimals) * decimals).astype(int)
    return lower.astype(str) + ' ~ ' + upper.astype(str)

# 2. 구간 설정 자리 수 설정
decimals = 100_000
target_cols = ['금액', '거래후잔액']

for col in target_cols:
    process_df[col] = make_range_str(process_df[col], decimals)

# 범주화 전/후를 담은 결과 데이터프레임 생성
result = pd.DataFrame({
    '처리전 금액': clean_df['금액'],
    '금액': process_df['금액'],
    '처리전 거래후잔액': clean_df['거래후잔액'],
    '거래후잔액': process_df['거래후잔액'],
})

result.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>



* 2- `생년월일`은 현재 날짜에 따른 연령대로 범주화

```python
# 실행 시점의 '현재 날짜' 가져오기
ref_date = pd.Timestamp.now()

birth_month = process_df['생년월일'].dt.month
birth_day = process_df['생년월일'].dt.day
not_yet_birthday = (birth_month > ref_date.month) | ((birth_month == ref_date.month) & (birth_day > ref_date.day))
age = ref_date.year - process_df['생년월일'].dt.year - not_yet_birthday.astype(int)

process_df['생년월일'] = (age // 10 * 10).astype(str) + '대'

process_df = process_df.rename(columns={'생년월일': '연령대'})
process_df.rename(columns={'생년월일': '연령대'}, inplace=True)

# 범주화 전/후를 담은 결과 데이터프레임 생성
result = pd.DataFrame({
    '생년월일': clean_df['생년월일'],
    '연령대': process_df['연령대']
})

result.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>
