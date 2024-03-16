此仓库包含使用 mdBook 框架创建的书籍项目，演示了如何自动部署书籍到 GitHub Pages。

## 项目概览

本项目利用 mdBook，一个用于从 Markdown 文件创建现代在线书籍的工具。我们的目标是自动化构建和部署流程，以便每次更新内容时都能快速发布到 GitHub Pages 上。

## 如何使用

要在本地构建和预览书籍，请确保已安装 [Rust](https://www.rust-lang.org/) 和 mdBook。然后，克隆此仓库并运行以下命令：

```
clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
mdbook serve
```

打开浏览器访问 `http://localhost:3000` 查看书籍。

## 自动部署流程

通过 GitHub Actions 实现了 CI/CD 流程，自动将书籍部署到 GitHub Pages。以下是部署流程的概述：

1. **推送更改**：将更改推送到 `main` 分支。
2. **GitHub Actions 触发**：基于 `.github/workflows/deploy.yml` 中定义的工作流程，GitHub Actions 会自动触发构建过程。
3. **构建书籍**：GitHub Actions 会安装 mdBook，然后构建书籍内容。
4. **部署到 GitHub Pages**：构建完成后，书籍会自动部署到 GitHub Pages。

你可以在 GitHub 仓库的 "Actions" 选项卡下查看工作流程的执行情况。

## 关于 mdBook

mdBook 是一个用 Rust 编写的命令行工具和库，用于从 Markdown 文件创建在线书籍。它灵感来源于 GitBook，并且专为编写技术文档和教程而设计。mdBook 生成的书籍具有清晰的结构、美观的界面，并且支持多种格式的输出，包括静态网站和 PDF 文件。

mdBook 提供了多种功能，如：

- **支持 Markdown**：使用 Markdown 编写内容，简单易用。
- **多语言支持**：可以轻松地创建多语言书籍。
- **可配置**：通过 `book.toml` 配置文件，你可以定制书籍的各个方面，如主题、插件和输出格式。
- **测试代码片段**：可以测试书籍中的代码片段，确保代码的准确性。
- **搜索功能**：生成的书籍带有内置的搜索功能，方便读者查找信息。

mdBook 非常适合创建如编程语言文档、软件使用手册和教程等技术文档。它的使用范围从个人项目到大型企业都非常广泛。

### 学习资源

要了解更多关于 mdBook 的信息，包括如何安装、使用和配置 mdBook，你可以访问以下资源：

- **GitHub 仓库**：https://github.com/rust-lang/mdBook
- **官方文档**：https://rust-lang.github.io/mdBook/

这些资源提供了丰富的指南和示例，帮助你开始使用 mdBook 来创建和发布你自己的在线书籍。

## 项目定制

本项目对 mdBook 进行了以下定制：

#### Black 主题

位于`theme\css\variables.css:62`

#### 自定义导航

位于`theme\index.hbs:180`

#### 修改CSS 效果

位于`theme\css\chrome.css`

如果你不需要这些定制，可以简单地通过以下操作进行调整或删除：

- **删除主题**：将 `theme` 文件夹从你的项目中删除，mdBook 将回退到默认主题。
- **修改主题和效果**：在 `theme` 文件夹中更改 CSS 和 hbs文件，你可以调整导航样式和其他视觉效果以满足你的需求。

## mdBook 目录生成工具

在本项目中还开发了一个小工具，用于从文件夹结构的一系列 Markdown 文件自动生成 `SUMMARY.md` 目录。简化从已有md文件生成书籍项目和目录结构的工作。

该工具的使用方法和源代码可以在下面的链接找到：

- [mdBook-tools GitHub 仓库](https://github.com/lzzsG/mdBook-generate-directory)
