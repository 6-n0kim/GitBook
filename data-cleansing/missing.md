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

# 결측치 확인 및 처리

### **3-1. 결측치 확인**

```python
# 컬럼별 결측치 개수 확인
print(clean_df.isnull().sum())
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

***

이 실습 데이터에는 아래처럼 컬럼별로 결측치가 섞여 있습니다.

| 컬럼    | 결측치 수 | 비율 | 데이터 유형 |
| ----- | ----- | -- | ------ |
| 금액    | 300   | 3% | 수치형    |
| 거래후잔액 | 300   | 3% | 수치형    |
| 은행명   | 200   | 2% | 범주형    |
| 거래유형  | 200   | 2% | 범주형    |
| 생년월일  | 100   | 1% | 개인정보성  |
| 주소    | 100   | 1% | 개인정보성  |

### **3-2. 결측치 처리**

결측치는 컬럼의 **성격**에 따라 처리 방식이 달라집니다.

* **1) 수치형 컬럼** (`금액`, `거래후잔액`): 분포를 확인한 뒤 **평균(mean)** 또는 **중앙값(median)** 으로 대치
  * **중앙값**: 주어진 숫자들을 크기 순서대로 나열했을 때 정확히 가장 가운데에 위치하는 값
* **2) 범주형 컬럼** (`은행명`, `거래유형`): **최빈값** 으로 대치, 혹은 '결측' 카테고리로 별도 표시
  * **최빈값**: 주어진 자료 중에서 가장 자주 나타나는(빈도수가 높은) 값
* **3) 개인정보 컬럼** (`생년월일`, `주소`): 임의로 채우지 않고 해당 행을 삭제하거나 원본 소스에서 재확인 (잘못 추정한 개인정보를 채워 넣는 것 자체가 위험할 수 있음)

1\) 수치형 컬럼

평균은 이상치(outlier)에 민감하게 반응하는 반면, 중앙값은 이상치의 영향을 거의 받지 않습니다. 두 값을 정하는 기준은 **분포가 대칭적인지, 이상치가 있는지**이며, 판단 기준으로 **왜도(skewness)** 를 확인합니다.

* **왜도**는 데이터 분포가 평균을 중심으로 얼마나 좌우 비대칭인지 나타내는 통계적 지표입니다. 분포가 대칭인 정규분포의 왜도는 0입니다.

```python
print("금액 왜도:", clean_df['금액'].skew())
print("거래후잔액 왜도:", clean_df['거래후잔액'].skew())
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

| 컬럼    | 왜도(skewness) | 분포 특징                        | 권장 대치 방식        |
| ----- | ------------ | ---------------------------- | --------------- |
| 금액    | 약 1.09       | 오른쪽으로 다소 치우침                 | 중앙값 (평균도 참고 가능) |
| 거래후잔액 | 약 2.47       | 오른쪽으로 심하게 치우침 (소수의 고액 거래 존재) | 중앙값 권장          |

* `|왜도| < 0.5`: 거의 대칭 → 평균 사용 가능
* `0.5 ≤ |왜도| < 1`: 약간 치우침 → 상황에 따라 판단
* `|왜도| ≥ 1`: 심하게 치우침 → 중앙값 권장

`거래후잔액`처럼 왜도가 큰 컬럼에서 평균으로 결측치를 채우면, 소수의 초고액 거래 때문에 평균값 자체가 실제 '전형적인 잔액'보다 훨씬 크게 왜곡됩니다. 이런 경우 중앙값이 대다수 고객의 실제 잔액 수준을 더 잘 대표합니다.

```python
# 1. '금액' 또는 '거래후잔액'에 결측치가 있는 행의 인덱스 추출
missing_idx = clean_df[clean_df['금액'].isnull() | clean_df['거래후잔액'].isnull()].index

# 2. [대치 전] 결측치가 존재하는 행 출력
print(f"--- [대치 전] 결측치 발생 행 (총 {len(missing_idx)}건) ---")
print(clean_df.loc[missing_idx, ['금액', '거래후잔액']])

# 3. 대치에 사용할 중앙값 계산
금액_median = clean_df['금액'].median()
잔액_median = clean_df['거래후잔액'].median()

print(f"\n 적용될 대치값 -> 금액: {금액_median}, 거래후잔액: {잔액_median}")

# 4. 결측치 대치 및 정수형(int) 변환
clean_df['금액'] = clean_df['금액'].fillna(금액_median).astype(int)
clean_df['거래후잔액'] = clean_df['거래후잔액'].fillna(잔액_median).astype(int)

# 5. [대치 후] 동일한 행 출력해서 비교
print("\n--- [대치 후] 변환 완료된 동일 행 목록 ---")
print(clean_df.loc[missing_idx, ['금액', '거래후잔액']])
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

2\) 범주형 컬럼

```python
# 1. '은행명' 또는 '거래유형'에 결측치가 있는 행의 인덱스 추출
missing_idx = clean_df[clean_df['은행명'].isnull() | clean_df['거래유형'].isnull()].index

# 2. [대치 전] 결측치가 존재하는 행 출력
print(f"--- [대치 전] 범주형 결측치 발생 행 (총 {len(missing_idx)}건) ---")
print(clean_df.loc[missing_idx, ['은행명', '거래유형']])

# 3. 범주형 컬럼별 최빈값(mode) 계산
은행명_mode = clean_df['은행명'].mode()[0]
거래유형_mode = clean_df['거래유형'].mode()[0]

print(f"\n▶ 적용할 대치값(최빈값) -> 은행명: '{은행명_mode}', 거래유형: '{거래유형_mode}'")

# 4. 최빈값으로 결측치 대치
clean_df['은행명'] = clean_df['은행명'].fillna(은행명_mode)
clean_df['거래유형'] = clean_df['거래유형'].fillna(거래유형_mode)

# 5. [대치 후] 동일한 행 출력해서 비교
print("\n--- [대치 후] 변환 완료된 동일 행 목록 ---")
print(clean_df.loc[missing_idx, ['은행명', '거래유형']])
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

3\) 개인정보 컬럼

```python
# 1. '생년월일' 또는 '주소'에 결측치가 있는 행의 인덱스 추출
missing_idx = clean_df[clean_df['생년월일'].isnull() | clean_df['주소'].isnull()].index

# 2. [삭제 대상 출력] 삭제될 결측치 보유 행 확인
print(f"--- [삭제 대상] '생년월일' 또는 '주소' 결측 행 (총 {len(missing_idx)}건) ---")
print(clean_df.loc[missing_idx, ['생년월일', '주소']])

# 3. 삭제 처리 (데이터 왜곡 방지를 위해 개인정보 결측 행 제거)
before = len(clean_df)
clean_df = clean_df.dropna(subset=['생년월일', '주소']).reset_index(drop=True)
after = len(clean_df)

# 4. 삭제 결과 요약 출력
print("\n--- [삭제 완료] 처리 결과 요약 ---")
print(f"삭제 전: {before}행 → 삭제 후: {after}행 (삭제된 행: {before - after}개, {(before - after) / before * 100:.2f}%)")
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**왜 `생년월일`, `주소`는 평균/최빈값으로 채우지 않는가?**

다음 단계(Step 2. 비식별 처리)에서 `생년월일`, `주소` 같은 준식별자(quasi-identifier)를 활용해 k-익명성 등의 비식별 기법을 적용할 예정입니다. 이런 컬럼을 평균이나 최빈값으로 채우면 데이터 왜곡 문제가 생깁니다.

* `주소`를 최빈값으로 채우면, 서로 다른 사람들이 실제로는 살지 않는 **동일한 주소를 가진 것처럼** 인위적으로 뭉쳐서, 실제 지역별 분포·클러스터 구조를 왜곡시킵니다.
* `생년월일`을 평균/최빈값으로 채우면, **존재하지 않는 개인정보를 새로 만들어내는 것**과 같아서 이후 연령대 분석이나 비식별 처리 결과의 신뢰도를 떨어뜨립니다.

따라서 이 두 컬럼은 그럴듯한 값으로 대치하는 대신, **결측이 발생한 행 자체를 삭제**하는 방식으로 데이터 왜곡을 최소화합니다. 다만 삭제는 아래 조건을 만족할 때만 안전한 선택입니다.

* 결측 비율이 낮음 (일반적으로 5% 미만이면 무난)
* 결측이 특정 은행·거래유형 등 특정 집단에 몰려 있지 않고 무작위에 가까움
* 표본 크기가 충분히 커서 일부를 제외해도 대표성에 문제가 없음

이번 데이터는 결측 비율이 약 2%(생년월일 100개 + 주소 100개, 중복 없이 총 200행)로 삭제해도 안전한 범위입니다.
{% endhint %}
