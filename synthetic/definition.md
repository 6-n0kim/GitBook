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

# 합성 함수 정의

### 합성 데이터 생성 전 필요한 함수 정의

```python
# ── 합성 함수 정의 (synthpop 방식: 순차적 CART + 리프 내 실제값 무작위 추출) ──
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
from sklearn.preprocessing import OrdinalEncoder
import hashlib

SYNTH_SEED = 42
MIN_LEAF = 5  # K-익명성 k=5와 동일한 기준

# CART는 정수 처리만 되기 떄문에 문자열 컬럼을 정수로 변환
def encode_predictors(df_real, df_synth, cols):
    enc = OrdinalEncoder(handle_unknown='use_encoded_value', unknown_value=-1)
    enc.fit(df_real[cols]) # 실제 데이터 기준으로 카테고리 -> 숫자 규칙을 확정
    return enc.transform(df_real[cols]), enc.transform(df_synth[cols]) # 같은 규칙을 양쪽에 적용

# 같은 리프(leaf)에 들어간 조건이 비슷한 그룹들로 구성된 기증자 풀(donor_pool) 생성
def _leaf_donor_pools(leaf_ids, y_real):
    y_real = np.asarray(y_real)
    return {leaf: y_real[leaf_ids == leaf] for leaf in np.unique(leaf_ids)}

# 실제 데이터로 학습된 Tree를 학습 시킨 뒤, 합성될 데이터를 Tree에 넣고 리프에서 무작위로 정답을 뽑아 합성
def cart_synthesize(X_real, y_real, X_synth, kind='categorical', min_samples_leaf=MIN_LEAF, seed=SYNTH_SEED):
    Tree = DecisionTreeClassifier if kind == 'categorical' else DecisionTreeRegressor
    tree = Tree(min_samples_leaf=min_samples_leaf, random_state=seed)
    tree.fit(X_real, y_real) # 1) 실제 데이터로 학습
    pools = _leaf_donor_pools(tree.apply(X_real), y_real) # 2) 리프별 실제값 풀 구성
    leaf_synth = tree.apply(X_synth) # 3) 합성 행들이 도착하는 리프 확인
    local_rng = np.random.default_rng(seed)
    return np.array([local_rng.choice(pools[leaf]) for leaf in leaf_synth])
```
