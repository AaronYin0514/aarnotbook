# ipynb文件转文档

将 Jupyter Notebook (`.ipynb` 文件) 转换为 Markdown (`.md` 文件) 可以通过 `nbconvert` 工具来完成。`nbconvert` 是 Jupyter 提供的一个命令行工具，可以将 Jupyter Notebook 转换为各种格式，包括 HTML、Markdown、PDF 等。

### 使用 `nbconvert` 转换 `ipynb` 到 `md`

以下是步骤：

#### 1. 安装 Jupyter（如果还没有安装）

首先，确保你已经安装了 Jupyter。你可以使用 `pip` 来安装：

```bash
pip install jupyter
```

#### 2. 使用 `nbconvert` 进行转换

打开终端或命令提示符，然后运行以下命令将 `notebook.ipynb` 转换为 `notebook.md`：

```bash
jupyter nbconvert --to markdown notebook.ipynb
```

该命令会在当前目录生成一个名为 `notebook.md` 的 Markdown 文件。

### 示例

假设你有一个名为 `example.ipynb` 的 Jupyter Notebook 文件，你可以运行以下命令将其转换为 Markdown：

```bash
jupyter nbconvert --to markdown example.ipynb
```

转换完成后，当前目录下会生成一个名为 `example.md` 的文件。

### 高级用法

#### 1. 输出到指定目录

你可以使用 `--output-dir` 参数将输出文件保存到指定目录：

```bash
jupyter nbconvert --to markdown notebook.ipynb --output-dir ./markdown_files
```

#### 2. 指定输出文件名

使用 `--output` 参数可以指定输出文件名：

```bash
jupyter nbconvert --to markdown notebook.ipynb --output custom_name.md
```

#### 3. 将多个文件转换为 Markdown

你也可以使用通配符来转换多个文件：

```bash
jupyter nbconvert --to markdown *.ipynb
```

### 转换后的文件

转换后的 Markdown 文件将包含 Jupyter Notebook 中的所有 Markdown 文本和代码单元格。代码单元格的输出也会被包含在内（如果运行了 notebook 的话）。此外，任何嵌入在 Notebook 中的图像也会被提取出来并与 Markdown 文件一起保存。

### 处理图像和其他资源

`nbconvert` 将自动处理并提取 Notebook 中嵌入的图像和其他资源，并将它们保存到一个与 Markdown 文件同名的目录中。例如，`example.ipynb` 转换后，会生成 `example.md` 和 `example_files` 目录，其中包含所有嵌入的图像。

通过这些步骤，你可以轻松地将 Jupyter Notebook 转换为 Markdown 文件，用于文档编写、博客发布或其他目的。