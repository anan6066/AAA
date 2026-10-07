# 智能模型量化部署一体化平台 — 代码归档

本仓库保存项目的**打包快照**（不含 git 历史的单文件归档）。

## 文件

| 文件 | 说明 |
| --- | --- |
| `system-v0.3.0-amct.zip` | v0.3.0 完整代码（含嵌入式 Python 运行环境） |

## 这个版本是什么

**v0.3.0-amct** —— 阶段 ④ 换成华为 **AMCT** 真实量化的版本：

```
ONNX 模型
  → ① 敏感度分析（权重谱范数，Hessian 代理）
  → ② NSGA-II 帕累托方案搜索（pymoo）
  → ③ 用户选方案
  → ④ AMCT 量化（IFMR 激活量化 + 权重量化，纯 CPU）
       → AscendQuant/AscendDequant + INT8 权重
  → ⑤ CANN ATC 编译
  → model.om（真 INT8）
```

实测对比（MobileNetV2，全 INT8 方案）：

| | 上一版（FP16）| 本版（AMCT + INT8）|
| --- | --- | --- |
| `quantized.onnx` | 7.1 MB | **3.6 MB** |
| `model.om` | 9.5 MB | **6.5 MB** |

## 展开后怎么用

```bash
unzip system-v0.3.0-amct.zip -d system
cd system
# 本地开发（Windows + WSL）
cd backend && scripts/wsl.cmd serve
# 部署到 Linux 服务器
./deploy.sh check
API_KEY=$(openssl rand -hex 24) ./deploy.sh all
```

详见包内的 `README.md`（本地开发）和 `DEPLOY.md`（服务器部署）。

## 源码仓库

日常开发请用带完整 git 历史的仓库：
<https://github.com/anan6066/system>（标签 `v0.3.0-amct`）
