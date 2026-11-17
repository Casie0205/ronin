# RoNIN Ubuntu 26.04 CPU 部署说明

## 本地与远端

- 本地仓库：`D:\Code\RoNIN`
- GitHub fork：`https://github.com/Casie0205/ronin`
- 上游仓库：`https://github.com/Sachini/ronin`
- 构建环境：WSL2 Ubuntu 26.04.1 LTS
- Python 环境：`/home/admin/miniforge3/envs/ronin`

## 环境版本

| 组件 | 版本 |
|---|---|
| Python | 3.11.18 |
| PyTorch | 2.6.0+cpu |
| TorchVision | 0.21.0+cpu |
| NumPy | 1.26.4 |
| Numba | 0.61.2 |
| Pandas | 3.0.6 |

## 当前兼容性修改

1. 将已由 NumPy 移除的 `np.float` 和 `np.int` 替换为 Python 内置类型。
2. 对本地可信 checkpoint 的 `torch.load` 显式设置 `weights_only=False`。

## 验证命令

```bash
cd /mnt/d/Code/RoNIN
/home/admin/miniforge3/envs/ronin/bin/python -m compileall -q source
```

```bash
cd /mnt/d/Code/RoNIN/source
/home/admin/miniforge3/envs/ronin/bin/python ronin_resnet.py --help
/home/admin/miniforge3/envs/ronin/bin/python ronin_lstm_tcn.py --help
/home/admin/miniforge3/envs/ronin/bin/python ronin_body_heading.py --help
```

当前结果：源码编译通过，三个主要入口均可启动。

## 数据与模型

RoNIN 公共数据只发布约原数据的 50%。训练和测试前需要按仓库 README 下载数据，并把配置文件中的路径改为本地数据目录。当前阶段只完成代码与 Python 环境，不包含数据下载和模型精度复现。

## D 盘透传加密规则

Windows Git 或 Windows 编辑器写入 `.py` 后可能产生 `%TSD-Header-###%` 加密头。克隆和源码修改应在 WSL 中完成：

```bash
git clone https://github.com/Casie0205/ronin.git /mnt/d/Code/RoNIN
```

发生加密后，应从 Git 恢复文件并在 WSL 中重新修改。
