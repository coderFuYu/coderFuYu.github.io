---
title: pandoc如何自定义样式导出docx文档
date: 2022-08-30 10:30:00
tags: [pandoc,markdown]
categories: 教程
keywords: pandoc
top_img: https://oss.fycoder.top/Blog/pandocTopPic.png
cover: https://oss.fycoder.top/Blog/pandocTopPic.png
---

## 

pandoc是一个方便的文档格式转换工具，其中一个功能是将markdown文件转换为docx文件，但是它是使用的自带样式，如果需要自己规定样式就需要提供一个样式文件。

### 1.导出默认样式文件

```bash
 pandoc -o custom-reference.docx --print-default-data-file reference.docx
```

这会在终端打开的文件夹下导出pandoc默认的docx导出格式。

### 2.修改样式文件

使用Microsoft Word 或者WPS修改样式文件，注意，**此处不是修改文档中内容的格式，而是修改文档的预设样式**：

![image-20220830101100554](https://oss.fycoder.top/markdown/202208301011805.png)

刚开始不理解来来回回弄了好几次。

### 3.使用自定义的样式导出markdown

```
pandoc -s markdown.md --reference-doc custom-reference.docx -o m.docx
```

其中markdown.md是现有文件，m.docx是导出后的文件名。

或者使用typora集成好的功能，直接设置样式文件地址进行一键导出

![typora](https://oss.fycoder.top/markdown/202208301023290.png)