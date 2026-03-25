Python 的 `Path` 类（来自 `pathlib` 模块）是用于**面向对象地处理文件系统路径**的核心类，它提供了比传统 `os.path` 更直观、跨平台的路径操作方式。

## 核心作用

`Path` 类将路径从**纯字符串**转变为**对象**，封装了路径解析、文件属性查询和文件系统操作等方法，使代码更具可读性和可移植性。

## 主要功能分类

### 1. 路径构建与解析
- 使用 `/` 运算符拼接路径（替代 `os.path.join`）
- 获取文件名、扩展名、父目录等组件
- 解析绝对路径与相对路径

### 2. 文件系统查询
- 检查存在性 (`exists()`)、类型 (`is_file()`, `is_dir()`)
- 获取元数据（大小、修改时间等）
- 遍历目录 (`iterdir()`, `glob()`, `rglob()`)

### 3. 文件操作
- 创建、删除、重命名文件/目录
- 读写文本/二进制数据（`read_text()`, `write_bytes()` 等）
- 创建目录（`mkdir()`，支持递归创建）

### 4. 跨平台兼容
- 自动处理 Windows 的反斜杠与 Unix 的正斜杠
- 提供 `PurePath`（纯逻辑路径）和 `Concrete Path`（实际文件系统操作）的区分

## 基础示例

```python
from pathlib import Path

# 路径拼接
p = Path("/home/user") / "documents" / "file.txt"

# 文件检查与读取
if p.exists():
    content = p.read_text(encoding='utf-8')

# 遍历目录
for py_file in Path(".").glob("**/*.py"):
    print(py_file.name)
```

相比 `os.path` 的字符串操作，`Path` 对象的方法链式调用更符合现代 Python 的编程风格。