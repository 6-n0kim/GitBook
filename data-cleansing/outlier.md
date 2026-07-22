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

### 5. 이상치  확인

이상치는 무조건 삭제하는 것이 아니라, **도메인 지식으로 정상 범위인지 먼저 판단**해야 합니다. 예를 들어 마이너스 통장(대출 한도 내 잔액)이라면 음수가 정상일 수 있습니다.

```python
# 금액 관련 컬럼 기초 통계 확인
print(clean_df[['금액', '거래후잔액']].describe())

# 거래후잔액이 음수인 경우 확인 (계좌 특성상 오류인지, 마이너스 통장인지 도메인 판단 필요)
print(clean_df[clean_df['거래후잔액'] < 0])
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
