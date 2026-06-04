# Notebook 项目维护记录

## 2026-06-04 维护内容

### 修复的问题

1. **mkdocs.yml 配置修复**
   - 移除了不存在的文件引用：`Course/CS61A/Note/Modular.md`
   - 修正了文件名大小写：`interpreter.md` → `Interpreters.md`
   - 移除了错误的路径引用：`EE/cm/main.md`（目录不存在）
   - 删除了重复的 markdown_extensions 配置段

2. **完善 .gitignore**
   - 添加 `site/` 目录（构建输出，35MB）
   - 添加 `.cache/` 目录
   - 添加 Python 缓存文件：`*.pyc`, `__pycache__/`

3. **添加项目依赖管理**
   - 创建 `requirements.txt` 文件
   - 包含必需的依赖：
     - mkdocs-material
     - mkdocs-statistics-plugin
     - mkdocs-git-revision-date-localized-plugin

4. **完善空文件内容**
   - `docs/tag.md`：添加标签索引页面基本结构
   - `docs/Course/CS61A/Note/SQL.md`：添加基本模板

### 项目结构

```
notebook/
├── .github/workflows/ci.yml  # GitHub Actions 自动部署
├── mkdocs.yml                # MkDocs 配置文件
├── requirements.txt          # Python 依赖
├── .gitignore               # Git 忽略文件
├── docs/                    # 文档源文件
│   ├── CS/                  # 计算机科学
│   ├── EE/                  # 电气工程
│   ├── Course/              # 课程笔记
│   └── ...
├── material/                # 自定义主题
└── site/                    # 构建输出（已忽略）
```

### 构建和部署

**本地构建：**
```bash
pip install -r requirements.txt
mkdocs serve
```

**部署到 GitHub Pages：**
- 推送到 main/master 分支会自动触发 CI/CD
- GitHub Actions 会自动构建并部署到 gh-pages 分支

### 注意事项

1. `site/` 目录由 mkdocs 自动生成，不应提交到 Git
2. 添加新笔记后需要更新 `mkdocs.yml` 中的 nav 配置
3. 文件路径需要注意大小写（Windows 不区分，但 Linux 区分）
