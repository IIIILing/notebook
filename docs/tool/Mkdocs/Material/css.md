---
title: 一些好用的内置CSS
statistics: True
---

**MkDocs Material** 主题内置了许多漂亮的 CSS 布局和功能，适合快速构建精美文档页面。以下是一些常见的 **自带好看的布局**，以及如何使用它们的示例：

---

### Admonitions

Admonitions 是 MkDocs Material 的强大功能，提供了多种风格化框架，非常适合突出信息。

```markdown
!!! note "提示框标题（可选）"
    这是一个提示框，用于显示额外的信息。

!!! warning "警告框"
    请注意，这是一个警告框！

!!! success "成功信息"
    操作已成功完成。

!!! failure "失败信息"
    出现了一些错误。

!!! quote "引用内容"
    > 这是一个引述内容的示例。
```

- **Note**: 绿色边框，轻微背景高亮。
- **Warning**: 黄色背景和图标，适合警告信息。
- **Success**: 绿色成功图标，非常直观。
- **Failure**: 红色警告框，突出错误。

---

### Grid Layout

内置的网格布局支持内容以响应式方式排列，适合卡片布局或多栏展示。

```markdown
<div class="grid cards" markdown>
- **卡片 1**
  这是卡片内容。

- **卡片 2**
  这是另一个卡片内容。

- **卡片 3**
  更多的卡片内容。

- **卡片 4**
  这是一张额外的卡片。
</div>
```

- 网格卡片会自动适应屏幕大小。
- 在 CSS 中自定义 `grid-template-columns` 可以控制列数。

---

### Code Block

MkDocs Material 的代码块非常美观，支持高亮、折叠和复制功能。

```markdown
```python
def hello_world():
    print("Hello, world!")
```

- 支持多种语言的语法高亮。
- 自动添加 **复制代码按钮**（通过 `mkdocs.yml` 配置启用）。
- 提供 **折叠代码块** 的选项。

---

### Tabs

标签页功能非常适合在同一位置展示不同内容，如多语言代码或版本切换。

```markdown
=== "Python"
    ```python
    print("Hello, Python!")
    ```

=== "JavaScript"
    ```javascript
    console.log("Hello, JavaScript!");
    ```

=== "C++"
    ```cpp
    std::cout << "Hello, C++!" << std::endl;
    ```
```

- 在页面上展示为可切换的标签。
- 自动隐藏非选中内容，节省空间。

---

### Code Blocks with Line Numbers

内置行号样式，让代码更具可读性。

```markdown
```yaml linenums="1"
site_name: My Awesome Docs
theme:
  name: material
```

- 行号会显示在代码块的左侧，适合技术文档或教程。

---

### Footnotes

优雅的脚注样式，适合学术文档或详细说明。

```markdown
这是正文[^1]。

[^1]: 这是脚注内容。
```

- 脚注以上标数字表示，点击可跳转至说明。
- 页面底部自动生成脚注列表。

---

### cons图标

通过 Material Design Icons，快速添加漂亮的图标。

```markdown
:material-check-circle: 成功  
:material-alert-circle: 警告  
:material-information: 信息
```

- 图标直观美观，适合内容标识。
- 可通过 `mkdocs.yml` 配置加载更多图标。

---

### Task Lists

任务列表可以用来展示 TODO 或已完成的任务。

```markdown
- [x] 已完成的任务
- [ ] 待完成的任务
- [x] 另一个已完成任务
```

- 以复选框样式展示任务状态。
- 动态生成的内容清晰直观。

---

### Expandable Sections

可折叠的内容区域，适合隐藏额外信息。

```markdown
??? note "点击展开"
    这是隐藏的内容。
    你可以在这里写更多内容。
```

- 点击标题即可展开和隐藏内容。
- 非常适合 FAQ 或隐藏细节。

---

### Responsive Tables

表格布局自动适配移动端设备，保证在小屏幕上可读性。

```markdown
| Feature      | Description         |
|--------------|---------------------|
| Responsive   | 自适应布局         |
| Customizable | 支持自定义 CSS     |
| Beautiful    | 美观简洁           |
```

- 小屏幕下自动缩放表格或开启滚动。

---

### Buttons

通过内置按钮样式快速创建美观的交互元素。

```markdown
[按钮示例](#){ .md-button }
[带颜色的按钮](#){ .md-button .md-button--primary }
```

- `md-button`: 普通按钮样式。
- `md-button--primary`: 主按钮样式。

---
