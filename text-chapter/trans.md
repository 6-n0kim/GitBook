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

# 수동 변환 기능

### **6. 수동 변환 기능 및 적용**

* 탐지된 PII를 **직접 하나씩 확인**하고, 원하는 문자열을 **수동**으로 바꿉니다.
* 단순 삭제/마스킹 외에 **맞춤형** 비식별 처리가 필요할 때 유용합니다.

```python
import ipywidgets as widgets
from IPython.display import display

print("탐지된 개인정보 목록 (수정할 항목만 텍스트를 입력하세요):")

# 1. 각 PII 항목별 입력창 생성
widget_inputs = []
for i, (label, start, end, val) in enumerate(unique_spans):
    text_box = widgets.Text(
        value='',
        placeholder='변경할 값 입력',
        description=f'{i}. [{label}] "{val}" ->',
        style={'description_width': 'initial'},
        layout=widgets.Layout(width='80%')
    )
    widget_inputs.append((val, text_box))

# 2. 버튼 및 출력 영역 생성
apply_button = widgets.Button(
    description="치환 적용",
    button_style='primary',
    icon='check'
)
output_area = widgets.Output()

text_replaced = text

# 3. 버튼 클릭 시 실행될 이벤트
def apply_replacements(b):
    global text_replaced
    with output_area:
        output_area.clear_output()

        replacements = {}
        for orig_val, text_box in widget_inputs:
            new_val = text_box.value.strip()
            if new_val:
                replacements[orig_val] = new_val

        text_replaced = text
        for label, start, end, orig in sorted(unique_spans, key=lambda x: x[1], reverse=True):
            if orig in replacements:
                new = replacements[orig]
                text_replaced = text_replaced[:start] + new + text_replaced[end:]

        print("✅ 치환이 적용되었습니다\n")
        print("수정된 전체 텍스트:")
        print(text_replaced)

apply_button.on_click(apply_replacements)

# 4. 화면 출력
display(widgets.VBox([tb for _, tb in widget_inputs] + [apply_button]))
display(output_area)
```

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>
