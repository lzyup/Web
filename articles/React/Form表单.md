
### 常用表单总结
| Element                    | Value property      | Change callback | New value in the callback |
| -------------------------- | ------------------- | --------------- | ------------------------- |
| `<input type="text"/>`     | `value="string"`    | `onChange`      | `event.target.value`      |
| `<input type="checkbox"/>` | `checked={boolean}` | `onChange`      | `event.target.checked`    |
| `<input type="radio">`     | `checked={boolean}` | `onChange`      | `event.target.checked`    |
| `<textarea />`             | `value="string"`    | `onChange`      | `event.target.value`      |
| `<select />`               | `value=optionvalue` | `onChange`      | `event.target.value`      |

### 受控组件与非受控组件
在受控组件中，表单数据由React组件负责处理。在非受控组件中，其表单数据由DOM元素本身处理

