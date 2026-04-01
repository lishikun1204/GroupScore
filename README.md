# 小组积分管理系统（单机版）

本项目为 **Tkinter 单机桌面客户端**，面向 6 个小组的积分展示与管理，支持成员管理。程序可直接拷贝到 U 盘运行，也可打包为单文件 `.exe`。

## 功能概览

- 小组卡片展示：分数、颜色标识、快捷加减分
- 小组管理：新增/编辑/删除小组（含颜色、标识）
- 成员管理（全局独立列表）：成员新增/编辑/删除、调整所属组
- 响应式布局：1920 / 1440 / 768 / 375 四档断点适配
- 可用性反馈：内置反馈表单写入本地文件（便于收集意见）

## 运行方式（开发环境）

1. 安装依赖：

```bash
python -m pip install -r requirements.txt
```

2. 启动程序：

```bash
python main.py
```

可选参数：

```bash
python main.py --theme light
python main.py --theme dark
python main.py --font-scale 1.0
python main.py --disable-emoji
```

## 数据文件（便携运行）

- 程序数据默认使用 `scores_data.json`
- 打包为 exe 后，数据文件会在 exe 同级目录读取/写入（适合 U 盘便携）

建议部署结构：

```
GroupScore.exe
scores_data.json
```

## 成员管理

在菜单「小组 → 成员管理」中：
- 支持成员 CRUD 与调整所属组

## 设计规范

主页面视觉设计规范见 [DESIGN_SYSTEM.md](file:///c:/Users/Administrator/PycharmProjects/PythonProject/DESIGN_SYSTEM.md)。

## 可用性反馈与指标

- 菜单「帮助 → 可用性反馈」可提交反馈
- 输出文件（程序目录下）：
  - `perf_artifacts/feedback.jsonl`：可用性反馈（每行一条 JSON）
  - `perf_artifacts/metrics.jsonl`：首帧渲染与刷新耗时等基础指标

说明：Web 的 LCP/FID/CLS 不适用于 Tkinter 桌面应用；本项目使用桌面端等价指标替代。

## 打包为 exe（Windows）

建议在干净的 Python 环境中执行：

```bash
python -m pip install -r requirements.txt
python -m pip install --upgrade pyinstaller
python -m PyInstaller --noconsole --onefile --clean --name GroupScore main.py
```

输出：
- `dist/GroupScore.exe`

## Git 忽略与行尾

- 已提供 `.gitignore` 忽略打包产物、缓存与本地数据
- 如出现 LF/CRLF 提示，建议通过 `.gitattributes` 统一行尾策略（可按团队习惯配置）
