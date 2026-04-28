# 重庆大学本科毕业论文（设计）LaTeX 模板

这是一个面向重庆大学本科毕业论文（设计）的 LaTeX 模板，包含中英文封面、摘要、目录、正文、参考文献、附录、致谢和声明等常用组成部分。模板将论文格式配置集中在 `cquthesis.sty` 中，将个人与论文信息集中在 `metadata.tex` 中，便于按章节维护正文内容。

> 本模板为非官方模板，请在提交前以学校、学院和指导教师发布的最新格式要求为准。

## 功能特点

- 支持中文封面和英文封面。
- 支持中文摘要、英文摘要、目录、正文、参考文献、附录、致谢和声明页。
- 使用 GB/T 7714 数字顺序制参考文献样式。
- 内置论文常用页边距、页眉页脚、标题、目录、图表编号和字体设置。
- 随仓库提供 Times New Roman、宋体、黑体、楷体等字体文件，减少跨平台字体差异。

## 文件结构

```text
.
├── main.tex                 # 学校提交打印版入口，双面排版
├── main-screen.tex          # 电子阅读版入口，单面居中排版
├── metadata.tex             # 论文题目、作者、导师、学院、日期、关键词等信息
├── cquthesis.sty            # 模板格式配置
├── ref.bib                  # BibTeX 参考文献数据库
├── gbt7714-numerical.bst    # GB/T 7714 数字顺序制 BibTeX 样式
├── content/                 # 各章节和前后置页面
├── font/                    # 模板使用的字体文件
├── static/                  # 校徽、示例图片等静态资源
└── main.pdf                 # 示例编译结果
```

## 快速开始

### 1. 安装 LaTeX 发行版

推荐安装 TeX Live、MacTeX 或 MiKTeX，并确保可以使用 `xelatex`、`bibtex`。如果使用 `latexmk`，编译会更方便。

### 2. 克隆模板

```bash
git clone https://github.com/love-jr/cqu-thesis-latex-template.git
cd cqu-thesis-latex-template
```

### 3. 修改论文信息

编辑 `metadata.tex`，替换论文类型、题目、作者、学号、导师、专业、学院、提交日期和关键词等信息：

```tex
\newcommand{\cquTitleCN}{XXX计算方法的研究}
\newcommand{\cquTitleEN}{Study on Calculation Method of XXX}
\newcommand{\cquAuthorCN}{方\quad X}
\newcommand{\cquStudentID}{20XXXXXX}
```

如果没有助理指导教师，可以将相关字段保留为空：

```tex
\newcommand{\cquAssistantSupervisorCN}{}
\newcommand{\cquAssistantSupervisorEN}{}
```

### 4. 编写正文

正文内容位于 `content/` 目录。默认章节入口在 `main.tex` 和 `main-screen.tex` 中保持一致：

```tex
\input{content/1-section1.tex}
\input{content/2-section2.tex}
\input{content/3-section3.tex}
\input{content/4-section4.tex}
```

可以按需新增、删除或重命名章节文件，并同步修改 `main.tex` 中的 `\input{...}`。

### 5. 管理参考文献

将 BibTeX 条目写入 `ref.bib`，在正文中使用 `\cite{...}` 引用。参考文献页由 `content/Ref.tex` 生成，默认使用：

```tex
\bibliographystyle{gbt7714-numerical}
\bibliography{ref}
```

### 6. 编译论文

如果安装了 `latexmk`，推荐使用：

```bash
latexmk -xelatex main.tex
```

电子阅读版可编译为：

```bash
latexmk -xelatex main-screen.tex
```

也可以手动编译：

```bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

编译成功后会生成 `main.pdf`。

## 打印版与电子版

- `main.tex` 为学校提交打印版，默认采用 `twoside` 双面排版，并保留 10mm 装订线，适合按学校规范打印装订。
- `main-screen.tex` 为电子阅读版，采用 `oneside` 单面排版，并通过 `\usepackage[screen]{cquthesis}` 取消装订线，使各页版心保持居中。
- 两个入口共用同一套 `content/`、`metadata.tex` 和 `ref.bib`，因此正文内容只需要维护一份。

## 常用修改

### 单面或双面打印

`main.tex` 默认使用双面排版：

```tex
\documentclass[UTF8,a4paper,12pt,twoside,fontset=none]{ctexart}
```

如果只需要单面排版，可以删除 `twoside` 选项：

```tex
\documentclass[UTF8,a4paper,12pt,fontset=none]{ctexart}
```

如果希望保留模板的双版本结构，建议直接使用现成的 `main-screen.tex`，而不要手工改动 `main.tex`。

### 页眉标题

各章节通常在文件开头通过 `\cqusetheader{...}` 设置页眉右侧或奇数页页眉内容：

```tex
\cqusetheader{1 绪论}
```

新增章节时建议同步更新该标题。

### 图片资源

图片可放在 `static/` 目录，并使用 `graphicx` 的 `\includegraphics` 插入：

```tex
\begin{figure}
  \includegraphics[width=0.8\textwidth]{static/temp.PNG}
  \caption{示例图片}
  \label{fig:example}
\end{figure}
```

## 常见问题

### 编译时提示缺少字体

模板默认从 `font/` 目录加载字体文件。请确认克隆或上传项目时保留了整个 `font/` 目录。

### 目录或参考文献没有更新

LaTeX 需要多次编译才能更新目录、交叉引用和参考文献。建议使用 `latexmk -xelatex main.tex`，或按“手动编译”中的顺序运行命令。

### 在 Overleaf 中使用

将仓库完整上传到 Overleaf 后，在项目设置中选择 XeLaTeX 编译器。请确保 `font/`、`static/`、`content/` 和参考文献相关文件均已上传。

## 许可

当前仓库尚未包含 `LICENSE` 文件。使用、修改或分发本模板前，请先确认仓库维护者的授权方式。
