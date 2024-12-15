---
layout: post
title: Windows下tree命令详解
categories: [windows, tree, 命令, 工具]
description: tree命令是Windows下一个用于显示目录结构的工具，本文详细介绍了tree命令的用法，包括参数解析、实际案例、输出保存、注意事项等。
keywords: tree, windows, 命令, 工具
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

`tree` 命令是 Windows 下一个用于显示目录结构的工具。以下是详细的使用说明，包括参数解析、实际案例、输出保存、注意事项等：

---

## **命令语法**

```cmd
tree [路径] [/F] [/A]
```

**说明：**

- `路径`：要显示目录树的路径。默认是当前目录。
- `/F`：显示文件名，默认只显示文件夹。
- `/A`：用 ASCII 字符显示树状结构，适用于不支持图形字符的环境。

---

## **参数详解**

1. **路径**  
   指定要查看的目录。例如 `C:\Windows` 表示列出 Windows 文件夹的树状结构。

   - **不指定路径时**：默认列出当前目录结构。
   - **使用相对路径**：如 `tree .` 表示当前目录，`tree ..` 表示上一级目录。

2. **/F**  
   显示目录中的文件。默认情况下，只列出文件夹。

3. **/A**  
   使用 ASCII 字符（如 `|`, `-`）代替图形字符（如 `├`, `─`），方便在不支持图形字符的终端（如某些远程连接工具）中查看。

---

## **实际案例**

### **1. 列出当前目录的结构**

```cmd
tree
```

**输出：**

```plaintext
C:.
├───Documents
├───Downloads
└───Pictures
```

---

### **2. 列出指定目录的结构**

```cmd
tree C:\Windows
```

**输出：**

```plaintext
C:\Windows
├───Fonts
├───System32
│   ├───drivers
│   └───etc
└───Temp
```

---

### **3. 列出目录及其文件**

```cmd
tree /F
```

**输出：**

```plaintext
C:.
├───Documents
│       report.docx
│       notes.txt
├───Downloads
│       setup.exe
│       readme.md
└───Pictures
        image1.jpg
        image2.png
```

---

### **4. 列出指定目录的文件和目录**

```cmd
tree C:\Users /F
```

**输出：**

```plaintext
C:\Users
├───Public
│       desktop.ini
├───User1
│   ├───Desktop
│   │       shortcuts.lnk
│   └───Documents
│           paper.docx
└───User2
        todo.txt
```

---

### **5. 使用 ASCII 字符显示树状结构**

```cmd
tree /A
```

**输出：**

```plaintext
C:.
|---Documents
|---Downloads
|---Pictures
```

---

### **6. 使用 ASCII 字符和显示文件**

```cmd
tree /F /A
```

**输出：**

```plaintext
C:.
|---Documents
|       report.docx
|       notes.txt
|---Downloads
|       setup.exe
|       readme.md
|---Pictures
        image1.jpg
        image2.png
```

---

### **7. 保存输出到文件**

可以将结果重定向到文件：

```cmd
tree C:\ /F > directory_tree.txt
```

**说明：**

- 将树状结构输出到文件 `directory_tree.txt` 中，便于查看或分享。
- 文件会存储在当前命令行的工作目录中。

---

## **注意事项**

1. **命令执行时间**

   - 在大目录中运行 `tree` 命令可能会耗时较长，因为需要递归列出所有子目录和文件。

2. **中文字符显示问题**

   - 如果目录中包含中文文件名，可能会在某些编码设置下显示乱码。可以在运行命令前设置编码：
     ```cmd
     chcp 65001
     ```
     这会将控制台编码设置为 UTF-8。

3. **权限问题**

   - 如果尝试列出系统目录（如 `C:\Windows`），可能需要管理员权限才能查看所有子目录。

4. **输出内容较大**

   - 对于复杂的目录结构，建议结合 `| more` 分页查看，避免输出内容刷屏：
     ```cmd
     tree C:\ /F | more
     ```

5. **路径长度限制**
   - 如果路径太长（超过 260 个字符），可能会导致部分目录未列出。可以使用更长路径支持的工具代替 `tree`。

---

## **综合案例**

**列出 `D:\Projects` 的完整目录结构并保存为文件：**

```cmd
tree D:\Projects /F > projects_tree.txt
```

**逐页显示 `C:\Users` 的目录结构（包括文件）：**

```cmd
tree C:\Users /F | more
```

---

written by [曹斌](https://github.com/AaaBinfinity)
