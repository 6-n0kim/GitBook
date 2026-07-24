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

# 해시 함수

### 4. 해시 함수 처리 (SHA-256)

원본값에 솔트(Salt)를 결합한 뒤 해시 알고리즘(SHA-256)을 적용하여 복호화가 불가능한 고정 길이의 값으로 변환합니다.

* `계좌번호`  : 해시 함수

```python
import hashlib

# ── 계좌번호: 해시 함수(SHA-256) 기반 가명처리 ──
# salt는 외부에 노출되지 않도록 별도로 안전하게 관리해야 합니다.
SALT = "forte_practice_salt_2026"

def pseudonymize_account(acc, salt=SALT):
    return hashlib.sha256((acc + salt).encode()).hexdigest()[:16]  # 앞 16자리만 사용

process_df['계좌번호'] = process_df['계좌번호'].apply(pseudonymize_account)

# 해시 처리 전/후를 담은 결과 데이터프레임 생성
result = pd.DataFrame({
    '처리전 계좌번호': clean_df['계좌번호'],
    '처리후 계좌번호': process_df['계좌번호']
})

result.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

> ⚠️ **salt를 사용하는 이유**: salt 없이 `hash(계좌번호)`만 쓰면, 공격자가 가능한 계좌번호 조합을 미리 다 해시해둔 표와 대조해서 원본을 역산할 수 있습니다. salt를 붙이면 동일한 해시 알고리즘이라도 salt를 모르는 이상 역산이 사실상 불가능해집니다

