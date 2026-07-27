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

# 합성 적용 - 4. 식별자 처리

### 4. 식별자 처리 - CART 미적용

다른 컬럼과 통계적 관계가 없어 CART를 적용하지 않습니다. `이름`은 무작위 재추출, `계좌번호`는 새로운 일련번호로, `record_id`는 새 순번을 부여합니다.

```python
synth_df['이름'] = rng.choice(process_df['이름'].values, size=n, replace=True)
synth_df['계좌번호'] = [hashlib.sha256(f"synthetic_{i}_{SYNTH_SEED}".encode()).hexdigest()[:16] for i in range(n)]
max_id = process_df['record_id'].str.extract(r'(\d+)').astype(int).max()[0]
synth_df['record_id'] = [f"R{max_id + i + 1:05d}" for i in range(n)]

synth_df = synth_df[process_df.columns.tolist()]
synth_df.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>
