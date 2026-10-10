# YOLO11n 训练与推理

基于 Ultralytics YOLO11n 的目标检测训练与实时推理。

## 环境

- Python 3.14
- PyTorch 2.14.1+cu132（必须 CUDA 12.8+，否则 RTX 50 系显卡报 no kernel image）
- Ultralytics 8.4.175

## 硬件

- GPU：NVIDIA RTX 5070 Ti Laptop（12GB，sm_120）
- 实测推理速度：13.3ms/图

## 训练

```bash
python train.py
```

- 模型：yolo11n.pt
- 数据集：coco8.yaml（8 张图，官方迷你数据集）
- 参数：3 epochs，imgsz=640

## 推理

静态图片：

```bash
python predict.py
```

摄像头实时：

```bash
python webcam.py
```

## 训练结果

| 指标 | 数值 |
|---|---|
| mAP50 | 0.634 |
| mAP50-95 | 0.886 |
| 推理速度 | 13.3ms/图 |

## 踩坑记录

RTX 5070 Ti（sm_120）不兼容旧版 PyTorch

初始安装 torch 2.14.0+cu126，训练时报：

```
CUDA error: no kernel image is available for execution on the device
```

原因：PyTorch 2.14.0 的 CUDA 12.6 轮子不包含 sm_120 内核。解决方案：

```bash
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu132
```

GitHub 下载权重失败

yolo11n.pt 从 GitHub release 下载时 SSL 握手失败。解决方案：用镜像 https://ghfast.top/ 手动下载，放到脚本同目录。
