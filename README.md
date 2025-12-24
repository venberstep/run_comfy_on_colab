# 🎨 ComfyUI Colab 持久化部署方案

> 在 Google Colab 免费 GPU 环境中运行 ComfyUI，支持 Google Drive 持久化存储

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

## ✨ 特性

- 🚀 **一键部署**：快速在 Colab 免费 GPU 上运行 ComfyUI
- 💾 **持久化存储**：模型、自定义节点、输出文件保存到 Google Drive
- 🔌 **Manager 集成**：内置 ComfyUI-Manager，轻松管理节点和模型
- 🌐 **公网访问**：通过 cloudflared 隧道获取可分享的访问地址
- ♻️ **会话恢复**：重启后自动恢复之前的配置和节点

## 📁 项目结构

```
colab_ComfyUI/
├── ComfyUI.ipynb                    # 主启动文件（克隆程序 + 启动服务）
├── comfyui_colab_with_manager.ipynb # Manager 安装 + 节点依赖管理
├── modelsDownload.ipynb             # 模型下载工具
└── README.md                        # 本文件
```

## 📂 Google Drive 持久化架构

```
/content/drive/MyDrive/ComfyUI/     ← 持久化存储（重启后保留）
├── models/                          ← 模型文件（checkpoints, loras, vae...）
├── custom_nodes/                    ← 自定义节点 + ComfyUI-Manager
├── input/                           ← 输入文件（上传的图片等）
├── output/                          ← 生成的输出文件
└── user/                            ← 用户配置和工作流

/content/ComfyUI/                    ← 临时环境（每次重新克隆）
├── main.py                          ← ComfyUI 程序入口
└── ...                              ← 符号链接指向 Drive 目录
```

## 🚀 快速开始

### 首次使用

按以下顺序在 Google Colab 中运行：

| 步骤 | 文件 | 说明 |
|:----:|------|------|
| 1️⃣ | **comfyui_colab_with_manager.ipynb** | 安装 ComfyUI-Manager 到 Drive |
| 2️⃣ | **modelsDownload.ipynb** | 下载基础模型（SD1.5/SDXL/Flux 等） |
| 3️⃣ | **ComfyUI.ipynb** | 启动 ComfyUI 服务 |

### 日常使用

只需运行 **ComfyUI.ipynb** 即可！程序会自动：
- 克隆最新版 ComfyUI
- 挂载 Google Drive
- 创建符号链接到持久化目录
- 启动服务并生成访问地址

### 新会话启动（安装过新节点后）

如果上次会话中通过 Manager 安装了新的自定义节点，新会话启动前需要：

1. 先运行 **comfyui_colab_with_manager.ipynb**（安装节点的 pip 依赖）
2. 再运行 **ComfyUI.ipynb** 启动服务

> 💡 **提示**：节点源码已保存在 Drive，但 pip 依赖在临时环境中，每次新会话需重装。

## 📦 文件说明

### ComfyUI.ipynb
主启动文件，每次使用时运行。功能包括：
- 挂载 Google Drive
- 克隆 ComfyUI 到临时目录
- 创建持久化目录的符号链接
- 安装依赖并启动服务
- 通过 cloudflared 创建公网隧道

### comfyui_colab_with_manager.ipynb
Manager 和节点管理。功能包括：
- 安装/更新 ComfyUI-Manager
- 扫描并安装自定义节点的 Python 依赖
- 修复 Drive 上的脚本权限问题

### modelsDownload.ipynb
模型下载工具。支持：
- Stable Diffusion 1.5 / 2.1
- SDXL 及其 Refiner
- Flux 系列模型
- VAE、LoRA、ControlNet 等

## ⚠️ 注意事项

### 持久化范围

| 内容 | 是否持久化 | 说明 |
|------|:----------:|------|
| 模型文件 | ✅ | 保存在 Drive |
| 自定义节点源码 | ✅ | 保存在 Drive |
| 生成的图片 | ✅ | 保存在 Drive |
| 用户配置/工作流 | ✅ | 保存在 Drive |
| Python pip 包 | ❌ | 每次需重装 |
| ComfyUI 主程序 | ❌ | 每次重新克隆 |

### Colab 使用限制

- **免费版限制**：连续运行约 4-12 小时后会断开
- **GPU 配额**：免费用户有每日 GPU 使用限制
- **存储空间**：注意 Google Drive 免费 15GB 限制
- **空闲断开**：长时间无操作会自动断开

### 最佳实践

1. **及时保存工作流**：在断开前导出重要的工作流 JSON
2. **定期清理输出**：避免 Drive 空间不足
3. **使用 Colab Pro**：如需更长运行时间和更好的 GPU

## 🔧 故障排除

### 常见问题

**Q: 启动后无法访问 ComfyUI？**
- 检查 cloudflared 隧道是否正常启动
- 查看输出中的访问地址是否正确

**Q: 自定义节点报错缺少依赖？**
- 运行 `comfyui_colab_with_manager.ipynb` 重新安装依赖
- 或在 Manager 界面点击「Install Missing」

**Q: 模型加载失败？**
- 确认模型已下载到正确的 Drive 目录
- 检查模型文件是否完整（未被中断下载）

**Q: Drive 挂载失败？**
- 重新运行挂载单元格
- 检查 Google 账号授权

## 📄 许可证

本项目仅供学习和研究使用。

## 🔗 相关链接

- [ComfyUI 官方仓库](https://github.com/comfyanonymous/ComfyUI)
- [ComfyUI-Manager](https://github.com/ltdrdata/ComfyUI-Manager)
- [Google Colab](https://colab.research.google.com/)

---

<p align="center">
  Made with ❤️ for the AI Art Community
</p>
