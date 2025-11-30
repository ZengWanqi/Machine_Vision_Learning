[Ultralytics官网](https://docs.ultralytics.com/zh/)
[Ultralytics GitHub 仓库](https://github.com/ultralytics/ultralytics)

# 简介
- [YOLO11](https://docs.ultralytics.com/zh/models/yolo11/)是**广受赞誉**的**实时对象检测**和**图像分割模型**的最新版本。
- YOLOL11基于[深度学习](https://www.ultralytics.com/zh/glossary/deep-learning-dl)和[计算机视觉](https://www.ultralytics.com/zh/blog/everything-you-need-to-know-about-computer-vision-in-2025)。
- YOLO11能轻松适应从**边缘设备**到**云 API**的**不同硬件平台**。
# 快速入门
## [安装 Ultralytics](https://docs.ultralytics.com/zh/quickstart/)
你能通过 Pip、Conda、Git克隆 和 Docker 共 4 种方式来安装 Ultralytics。
- **Pip 安装（推荐）**
  - python版本：python3.8 ~ 3.12
  - 可以通过运行以下命令，从 PyPI **安装或更新 ultralytics**：
  `pip install -U ultralytics`
  -U 是 --upgrade 的简写形式，它的作用是升级指定的包到最新版本。
  PYPI，即“Python 包索引”，它是 Python 官方的软件包仓库。
- **Conda 安装**
  - 可以通过运行以下命令，从 Conda 社区维护的软件包仓库 **安装 ultralytics**：
  `conda install -c conda-forge ultralytics`
  -c 用来指定**包的来源渠道**
  conda-forge是具体的渠道名称，即Conda 生态中的一个社区维护的软件包仓库。
  - 如果要在 **CUDA 环境**中安装，最好是在一行命令中同时安装 ultralytics，pytorch 和 pytorch-cuda。这样的话 conda 包管理器能解决任何冲突。
  `conda install -c pytorch -c nvidia -c conda-forge pytorch torchvision pytorch-cuda=11.8 ultralytics`
- **Git 克隆**
  - 可以通过运行以下命令，以**可编辑模式**来 **安装 ultralytics**：
  ```
  git clone https://github.com/ultralytics/ultralytics
  cd ultralytics
  pip install -e .
  ```
- **Docker**
  [ultralytics Docker Hub](https://hub.docker.com/r/ultralytics/ultralytics)
  使用 Docker 执行 ultralytics 它以**隔离容器**中的**软件包形式**存在，确保**在各种环境中性能一致**。
  Ultralytics 提供 5 个主要支持的 Docker 镜像，每个镜像都具有**高兼容性**和**效率**：
  
  - **Dockerfile**: 推荐用于训练的 GPU 镜像。
  - **Dockerfile-arm64**: 针对 ARM64 架构进行了优化，适用于在 Raspberry Pi 和其他基于 ARM64 的平台等设备上部署。
  - **Dockerfile-cpu**: 基于 Ubuntu 的 CPU 版本，适用于推理和没有 GPU 的环境。
  - **Dockerfile-jetson**: 专为 [NVIDIA Jetson](https://docs.ultralytics.com/zh/guides/nvidia-jetson/) 设备量身定制，集成了为这些平台优化的 GPU 支持。
  - **Dockerfile-python**: 仅包含 python 和必要依赖项的最小镜像，非常适合轻量级应用程序和开发。
  - **Dockerfile-conda**： 基于 Miniconda3，并使用 conda 安装了 ultralytics 软件包。
  
  以下是**获取最新镜像**并执行它的命令：
  ```
  t=ultralytics/ultralytics:latest
  sudo docker pull $t
  sudo docker run -it --ipc=host --runtime=nvidia --gpus all $t
  sudo docker run -it --ipc=host --runtime=nvidia --gpus '"device=2,3"' $t
  ```
  有关高级 Docker 用法，请浏览 [Ultralytics Docker 指南](https://docs.ultralytics.com/zh/guides/docker-quickstart/)。
  请参阅ultralytics [pyproject.toml](https://github.com/ultralytics/ultralytics/blob/main/pyproject.toml) 文件以获取**依赖项**列表。
## Ultralytics 的 Python 用法
- [Ultralytics YOLO Python 用法文档](https://docs.ultralytics.com/zh/usage/python/)能帮助你将 Ultralytics YOLO 无缝集成到 Python 项目中，以实现[目标检测](https://www.ultralytics.com/zh/glossary/object-detection)、[实例分割](https://docs.ultralytics.com/zh/tasks/segment/)和[图像分类](https://docs.ultralytics.com/zh/tasks/classify/)。

- 在本小节，你将学习如何加载和使用**预训练模型**、**训练新模型**以及**图像预测**。

- 以下代码可以实现**加载模型**、**训练模型**、**评估模型性能**和[**导出为 ONNX 格式**](https://docs.ultralytics.com/zh/modes/export/)。

```
from ultralytics import YOLO

# 通过配置文件初始化一个全新的 YOLOv11 模型
model = YOLO("yolo11n.yaml")

# 加载预训练模型 (推荐)
model = YOLO("yolo11n.pt")

# 使用'coco8.yaml'数据集训练 3 轮
results = model.train(data="coco8.yaml", epochs=3)

# 在数据集中的验证集上评估模型的性能
results = model.val()

# 使用该模型对图像进行物体检测
results = model("https://ultralytics.com/images/bus.jpg")

# 将模型导出为 ONNX 格式
success = model.export(format="onnx")
```
### 训练
[**训练模式**](https://docs.ultralytics.com/zh/modes/train/)用于在**自定义数据集**上训练 YOLO 模型。在此模式下，模型使用**指定数据集**和**超参数**进行训练。训练过程包括**优化模型参数**，使其能准确预测图像中物体类别和位置。
- **从预训练模型开始训练（推荐）**
```
from ultralytics import YOLO

model = YOLO("yolo11n.pt")  # 加载预训练模型权重文件
results = model.train(epochs=5)
```
- **从头开始训练**
```
from ultralytics import YOLO

model = YOLO("yolo11n.yaml")  # 加载模型配置文件
results = model.train(data="coco8.yaml", epochs=5)  # 指定数据集和训练轮次
```
- **恢复训练**
```
model = YOLO("last.pt")  # 加载最近一次保存的模型
results = model.train(resume=True)  # 恢复训练
```
### 验证
[**验证模式**](https://docs.ultralytics.com/zh/modes/val/)用于在 YOLO 模型训练完成后对其进行验证。在此模式下，模型会在验证集上进行**评估**，以衡量其[**准确性**](https://www.ultralytics.com/zh/glossary/accuracy)和**泛化能力**。此模式可用于调整模型的超参数，以提高其性能。
- **训练后验证**
```
from ultralytics import YOLO

# 加载 YOLO 模型
model = YOLO("yolo11n.yaml")

# 训练模型
model.train(data="coco8.yaml", epochs=5)

# 在训练集的验证集上验证
model.val()
```
- **在其他数据集上验证**
```
from ultralytics import YOLO

# 加载 YOLO 模型
model = YOLO("yolo11n.yaml")

# 训练模型
model.train(data="coco8.yaml", epochs=5)

# 在独立数据上进行验证
model.val(data="path/to/separate/data.yaml")
```
### 预测
[**预测模式**](https://docs.ultralytics.com/zh/modes/predict/)用于使用经过训练的 YOLO 模型**对新图像或视频进行预测**。在此模式下，模型从**检查点文件**加载，用户可以提供图像或视频来执行推理。该模型**预测输入图像或视频中对象的类别和位置**。
- **预测的具体实现**
```
import cv2
from PIL import Image
from ultralytics import YOLO

model = YOLO("model.pt")

results = model.predict(source="0")
results = model.predict(source="folder", show=True)

im1 = Image.open("bus.jpg")
results = model.predict(source=im1, save=True)

im2 = cv2.imread("bus.jpg")
results = model.predict(source=im2, save=True, save_txt=True) 
results = model.predict(source=[im1, im2])
```
- **预测结果**的解析

## Ultralytics 设置
你可以轻松访问和修改 Ultralytics 库的设置。
- 查看设置
```
from ultralytics import settings

# 查看所有设置
print(settings)

# 获取特定设置值
value = settings["runs_dir"]
```
- 修改设置
```
from ultralytics import settings

# 修改单个设置
settings.update({"runs_dir": "/path/to/runs"})

# 修改多个设置
settings.update({"runs_dir": "/path/to/runs", "tensorboard": False})

# 重新配置为默认值
settings.reset()
```
