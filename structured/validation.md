---
icon: check-circle
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

# 결과 검증

비식별화 결과를 검증하는 단계입니다.

## 1. 비식별화 결과 확인

```python
# 비식별화된 데이터 확인
print(df[['이름', '이름_해시', '전화번호', '전화번호_마스킹']].head(10))
```

## 2. 통계 검증

```python
# 비식별화 전후 비교
print("=== 비식별화 전 ===")
print(f"총 행 수: {len(df)}")
print(f"고유 이름 수: {df['이름'].nunique()}")
print(f"고유 전화번호 수: {df['전화번호'].nunique()}")

print("\n=== 비식별화 후 ===")
print(f"총 행 수: {len(df)}")
print(f"고유 이름 해시 수: {df['이름_해시'].nunique()}")
print(f"고유 마스킹 전화번호 수: {df['전화번호_마스킹'].nunique()}")
```

## 3. 재식별 위험 평가

```python
# 동일한 해시값을 가진 이름 확인 (해시 충돌 가능성)
hash_duplicates = df[df.duplicated(['이름_해시'], keep=False)]
print(f"해시 충돌 가능성 있는 행: {len(hash_duplicates)}")

# 마스킹된 전화번호 패턴 확인
print("\n마스킹된 전화번호 샘플:")
print(df['전화번호_마스킹'].head(10).tolist())
```

## 4. 데이터 내보내기

```python
# 비식별화된 데이터 저장
df.to_csv('/content/drive/MyDrive/anonymized_data.csv', index=False)
df.to_excel('/content/drive/MyDrive/anonymized_data.xlsx', index=False)

print("비식별화된 데이터가 저장되었습니다.")
```

## 5. 검증 체크리스트

- [ ] 모든 민감 정보가 비식별화되었는가?
- [ ] 비식별화된 데이터로 원본을 재구성할 수 없는가?
- [ ] 통계 분석이 가능한 수준인가?
- [ ] 데이터 무결성이 유지되는가?
