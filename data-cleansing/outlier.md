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

# ④ 이상치(Outlier) 확인

```python
# 금액 관련 컬럼 기초 통계 확인
print(df[['금액', '거래후잔액']].describe())

# 거래후잔액이 음수인 경우 확인 (계좌 특성상 오류인지, 마이너스 통장인지 도메인 판단 필요)
print(df[df['거래후잔액'] < 0])
```

\[출력 결과]

```
           금액    거래후잔액
count    9800     9800
mean   709871  1142521
std    843685  1679385
min      1000 -5369320
25%     25270    28240
50%    323936   395465
75%   1194233  1725610
max   2999526 15181411

  record_id   이름                 주소        생년월일   은행명           계좌번호  \
4    R00005  최예은  부산광역시 분당구 문화로 117  1987-09-24  하나은행  479-55-319684   

                 거래일시 거래유형      금액    거래후잔액  
4 2025-02-18 17:39:16   송금  335671 -5369320  
```

{% hint style="info" %}
이상치는 무조건 삭제하는 것이 아니라, **도메인 지식으로 정상 범위인지 먼저 판단**해야 합니다. 예를 들어 마이너스 통장(대출 한도 내 잔액)이라면 음수가 정상일 수 있습니다.
{% endhint %}
