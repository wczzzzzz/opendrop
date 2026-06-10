# 环境搭建说明

记录 OpenDrop 的本地开发环境如何搭建，免得日后忘记。

## 环境概况

- **虚拟环境**：`path/to/opendrop/.venv`，Python **3.9.6**
- **opendrop**：以**可编辑模式（editable）**安装，即 `pip install -e .`。
  改 `opendrop/*.py` 源码会**立即生效**，无需重装。
- **系统依赖**：Homebrew 安装的 **libarchive 3.8.7**（macOS 自带版本太旧，
  opendrop 会自动把 `DYLD_LIBRARY_PATH` 指向 Homebrew 版）。

## 从零重建步骤

```bash
# 1) 系统依赖（macOS 自带 libarchive 太旧，必须装新版）
brew install libarchive

# 2) 进项目目录，建虚拟环境
cd path/to/opendrop
python3 -m venv .venv
source .venv/bin/activate

# 3) 以可编辑模式安装 opendrop（含运行依赖）
pip install -e .

# 4)（可选）开发用依赖：格式化 / 检查 / 测试
pip install -r requirements-dev.txt
```

## 关键依赖版本（实测）

| 包 | 版本 | 备注 |
|---|---|---|
| **libarchive-c** | 5.3 | 5.x 改了 API，对应 `util.py` 的适配 |
| **Pillow** | 11.3.0 | 10+ 删除了 `ANTIALIAS`，对应 `util.py` 改用 `LANCZOS` |
| zeroconf | 0.148.0 | `setup.py` 要求 `>=0.24.2` |
| fleep / ifaddr | 1.0.1 / 0.2.0 | |
| requests / requests-toolbelt | 2.32.5 / 1.0.0 | |
| black / isort / flake8 / pylint / pytest | dev 工具 | 来自 `requirements-dev.txt` |

## 日常使用

```bash
# 每次新开终端，先激活环境
source path/to/opendrop/.venv/bin/activate

# 然后直接用
opendrop find
opendrop send -r <id> -f ./文件
opendrop receive
```

## 注意事项

1. **不用重装**：editable 安装下源码改动直接生效。只有改了 `setup.py` 的
   依赖清单时才需要重新 `pip install -e .`。
2. **macOS 不需要 OWL**：本机原生有 AWDL（`awdl0` 接口），OWL 只有 Linux 才需要。
3. **libarchive 找不到时**：手动指向 Homebrew 路径：
   ```bash
   export DYLD_LIBRARY_PATH="$(brew --prefix libarchive)/lib"
   ```
