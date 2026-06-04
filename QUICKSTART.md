# Notebook 快速使用指南

## 本地开发

### 安装依赖
```bash
pip install -r requirements.txt
```

### 本地预览
```bash
mkdocs serve
```
访问 http://127.0.0.1:8000

### 构建静态站点
```bash
mkdocs build
```

## 添加新笔记

1. 在 `docs/` 目录下创建或编辑 `.md` 文件
2. 在 `mkdocs.yml` 的 `nav` 部分添加导航链接
3. 使用 frontmatter 添加元数据：
   ```yaml
   ---
   title: "笔记标题"
   tags:
     - 标签1
     - 标签2
   ---
   ```

## 项目结构

- `docs/` - 所有笔记源文件
  - `CS/` - 计算机科学相关笔记
  - `EE/` - 电气工程相关笔记
  - `Course/` - 课程学习笔记
  - `Tool/` - 工具使用笔记
- `material/overrides/` - 自定义主题文件
- `mkdocs.yml` - 站点配置文件
- `.github/workflows/ci.yml` - 自动部署配置

## 自动部署

推送到 `main` 分支后，GitHub Actions 会自动：
1. 安装依赖
2. 构建站点
3. 部署到 GitHub Pages

## 注意事项

- 文件路径区分大小写
- 图片建议放在对应文件夹下
- `site/` 目录是自动生成的，不要手动修改
- 添加新页面后记得更新导航配置
