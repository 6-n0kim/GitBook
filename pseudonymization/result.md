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

# 비식별처리 결과

### 비식별처리 전체 결과

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

<table data-search="false"><thead><tr><th>컬럼</th><th>처리 방식</th></tr></thead><tbody><tr><td><code>이름</code></td><td>마스킹 (성 첫 글자만 남김)</td></tr><tr><td><code>주소</code></td><td>부분삭제 (시 단위만 남김)</td></tr><tr><td><code>record_id</code></td><td>순차 코드 매핑 (R00001...)</td></tr><tr><td><code>계좌번호</code></td><td>해시함수 (SHA-256 + salt)</td></tr><tr><td><code>금액</code>, <code>거래후잔액</code></td><td>범주화 (10만원 단위 구간)</td></tr><tr><td><code>생년월일</code> → <code>연령대</code></td><td>범주화 (10년 단위 연령대)</td></tr><tr><td><code>은행명</code>, <code>거래유형</code>, <code>거래일시</code></td><td>그대로 유지</td></tr></tbody></table>
