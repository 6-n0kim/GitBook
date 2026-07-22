---
description: csv 관련 라이브러리 다운로드 및 버전
---

# 필요한 라이브러리 다운로드

```python
!pip install pandas==2.2.2 numpy==2.0.2
```



```python
import pandas as pd
import numpy as np

# 큰 숫자가 지수표기법(예: 1.15e+06)으로 표시되지 않도록 출력 형식 고정
pd.set_option('display.float_format', '{:.0f}'.format)
```

