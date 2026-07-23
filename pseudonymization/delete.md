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

# 부분 삭제

### 2. 부분 삭제 처리

식별 가능성이 높은 특정 항목이나 정보의 일부 구성 요소를 아예 삭제하여 식별 위험을 낮춥니다.

```python
# ── 주소: 부분삭제 방식 (앞 2개 토큰만 남기고 나머지 삭제) ──
def partial_delete_address(addr):
    tokens = addr.split()
    return ' '.join(tokens[:2])

process_df['주소'] = process_df['주소'].apply(partial_delete_address)

# 부분삭제 전/후를 담은 결과 데이터프레임 생성
result = pd.DataFrame({
    '처리전 주소': clean_df['주소'],
    '주소': process_df['주소']
})

result.head(5)
```

\[출력 결과]

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>
