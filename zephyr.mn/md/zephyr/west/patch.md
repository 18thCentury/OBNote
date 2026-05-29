
---

# Zephyr 协作技巧：利用 `west patch` 管理第三方模块改动

## 1. 背景：为什么需要 `west patch`？

在 Zephyr 开发中，我们经常需要修改 `modules` 下的代码（如 `cherryusb`、`mcuboot` 等）。

- **直接修改的问题：** 当执行 `west update` 升级 SDK 时，你的本地修改会被覆盖或产生严重的 Git 冲突。
    
- **West Patch 的优势：** 它将你的修改以 `.patch` 文件形式保存在应用仓库中，通过自动化脚本在代码下载后自动“打补丁”，实现**业务代码与 SDK 代码的解耦**。
    

---

## 2. 核心文件结构

补丁管理通常遵循 **T2 Star Topology**（星型拓扑），将补丁维护在 `application` 仓库内。

Plaintext

```
west-workspace/
├── application/             # 你的主程序仓库
│   ├── west.yml             # 项目清单
│   └── zephyr/
│       ├── patches/         # 存放所有补丁文件
│       │   └──cherryusb_fix.patch  # 这个名称和或路径要和patch.yml 对应
│       └── patches.yml      # 补丁配置文件（核心）
└── modules/lib/cherryusb/   # 目标被修改的模块（由 west 自动下载）
```

注: 名称和或路径要和patch.yml 对应是指:
	如果 patches目录下 是modules/lib/cherryusb/cherryusb_fix.patch 
	那么 yaml:
	```
	patches:
	  - path: modules/lib/cherryusb/cherryusb_fix.patch
	```

---

## 3. 标准操作流程

### 第一步：生成补丁文件

在修改完模块代码后，进入该模块目录生成补丁：

PowerShell

```
cd modules/lib/cherryusb
# 建议使用 --no-prefix 减少路径层级麻烦，HEAD 导出所有改动
git diff HEAD > cherryusb_fix.patch
```

### 第二步：配置 `patches.yml`

在 `application/zephyr/` 下创建或编辑 `patches.yml`。

**注意：** `path` 是相对于 `patches.yml` 的路径；`module` 必须匹配 `west.yml` 中的项目名。

YAML

```
patches:
  - path: cherryusb_fix.patch
    sha256sum: <通过 certutil 或 sha256sum 获取的小写哈希>
    module: modules/lib/cherryusb
    apply-command: git apply
    author: Your Name
    comments: |
      修复了 CherryUSB 在 Zephyr 环境下的网络枚举问题。
```

### 第三步：自动化应用

新员工拉取代码后，只需执行：

1. `west init` / `west update`（获取原始代码）
    
2. **`west patch apply`**（自动应用所有补丁）
    

---

## 4. 常见问题与排查 (Troubleshooting)

### Q1: 执行 `west patch apply` 显示 `0 patches applied`？

这通常不是报错，而是插件认为“不需要工作”。

- **原因1：已应用过。** West 会记录状态。尝试运行 `west patch clean` 后再 apply。
    
- **原因2：代码不干净。** 如果你手动改了代码没撤销，West 为了安全会跳过。请确保模块处于 `git checkout .` 后的干净状态。

### Q3: 校验失败 (SHA256 mismatch)？

west 计算 sha256 会将crlf 替换为lf,再计算,所以会不匹配.

- **Windows 获取哈希：** `Get-FileHash fix.patch -Algorithm sha256`
    
- **Linux 获取哈希：** `sha256sum <文件>`
    

---

## 5. 最佳实践总结

1. **定期清理：** 升级 Zephyr 版本前，务必先执行 `west patch clean`。
    
2. **原子补丁：** 一个补丁只解决一个问题，方便后续上游合并后删除。
    
3. **提交补丁：** 永远记得把 `.patch` 文件和 `patches.yml` 的改动提交到 `application` 仓库。
    
4. **手动验证：** 如果 `west patch` 逻辑卡住，先进入模块目录用 `git apply --check <路径>` 手动验证补丁的有效性。
    

---

> **提示：** 记住，`west patch` 是一个辅助工具，它的底层依然是 `git apply`。遇到问题，先确保手动能打上补丁，再配置自动化。