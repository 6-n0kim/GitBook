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

# 이상치 확인

### 6. 이상치  확인

이상치는 무조건 삭제하는 것이 아니라, **도메인 지식으로 정상 범위인지 먼저 판단**해야 합니다. 예를 들어 마이너스 통장(대출 한도 내 잔액)이라면 음수가 정상일 수 있습니다.

```python
# 1. 수치형 데이터 이상치 탐색
print("=== 금액 관련 컬럼 기초 통계 확인 ===")
print(clean_df[['금액', '거래후잔액']].describe())

# 거래후잔액이 음수인 경우 확인 (계좌 특성상 오류인지, 마이너스 통장인지 도메인 판단 필요)
negative_balance_df = clean_df[clean_df['거래후잔액'] < 0]
print(f"\n=== 음수 잔액 데이터 확인 ({len(negative_balance_df)}건) ===")
print(negative_balance_df)

# 2. 날짜 데이터 논리 오류 탐색
# 날짜 비교를 위해 문자열을 datetime 형식으로 변환 (오류 발생 시 NaT로 처리)
clean_df['temp_거래일시'] = pd.to_datetime(clean_df['거래일시'], errors='coerce')
clean_df['temp_생년월일'] = pd.to_datetime(clean_df['생년월일'], errors='coerce')

# 거래일시가 생년월일보다 과거인 데이터 추출 (출생 전 거래)
invalid_date_df = clean_df[clean_df['temp_거래일시'] < clean_df['temp_생년월일']]

print(f"\n=== 날짜 논리 오류 데이터 확인 (거래일시 < 생년월일) ({len(invalid_date_df)}건) ===")
print(invalid_date_df[['이름', '생년월일', '거래일시', '거래후잔액']])

# 탐색이 끝난 후 임시 생성한 datetime 컬럼 삭제
clean_df = clean_df.drop(columns=['temp_거래일시', 'temp_생년월일'])

# 탐색된 데이터를 삭제 처리하고 싶을 경우 아래 코드 사용
# clean_df = clean_df[~(clean_df['거래후잔액'] < 0)]  # 음수 잔액 삭제
# clean_df = clean_df[~(pd.to_datetime(clean_df['거래일시']) < pd.to_datetime(clean_df['생년월일']))] # 날짜 오류 삭제
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>
