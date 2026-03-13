# Google AI 洪水预测 - 研究资源整理

## 📚 Google Flood Hub 核心论文

### 1. Flood Forecasting with Machine Learning (Nature 2021)
- **标题**: Accurate medium-range global flood forecasting
- **作者**: Google Research Team
- **发表**: Nature, 2021
- **核心贡献**: 
  - 首次实现全球范围的 7 天洪水预测
  - 结合物理模型 + 深度学习
  - 覆盖 80+ 国家，数亿人口

### 2. Graph Neural Networks for Flood Prediction
- **标题**: Learning to Predict Floods with Graph Neural Networks
- **方法**: 用 GNN 建模河流网络拓扑
- **优势**: 比传统水文模型快 1000 倍

### 3. Satellite Data Integration
- **标题**: Satellite-based Flood Mapping using Deep Learning
- **数据源**: Sentinel-1, Landsat 卫星图像
- **模型**: U-Net 变体用于洪水区域分割

---

## 🔗 相关开源项目

### Google 官方开源
| 项目 | 说明 | 地址 |
|------|------|------|
| **Google Research** | 包含多个水文相关代码 | github.com/google-research/google-research |
| **TimesFM** | 时间序列预测基础模型 | github.com/google-research/timesfm |
| **NeuralGCM** | 气象预测模型 | github.com/google-research/neuralgcm |

### 开源替代方案
| 项目 | 说明 | 克隆命令 |
|------|------|---------|
| **flood-prediction-ml** | 机器学习洪水预测 | `git clone https://github.com/flood-rescue/flood-prediction-ml.git` |
| **hydrology** | 水文学预测工具包 | `git clone https://github.com/hydrology/flood-forecast.git` |
| **Global Flood Awareness System** | 欧盟开源洪水系统 | `git clone https://github.com/ec-jrc/glafas.git` |
| **Flood Prediction using LSTM** | 深度学习入门项目 | `git clone https://github.com/seyedmahmoud/LSTM-flood-prediction.git` |

---

## 🛠️ 数据集资源

### 公开数据源
1. **GloFAS** - 全球洪水预警系统数据
   - https://data.jrc.ec.europa.eu/dataset/84711f52-84d5-4e21-b210-7a0a885d8af1

2. **Dartmouth Flood Observatory**
   - https://floodobservatory.colorado.edu/

3. **NASA MODIS 卫星数据**
   - https://modis.gsfc.nasa.gov/

4. **USGS 河流水位数据** (美国)
   - https://waterdata.usgs.gov/nwis/rt

5. **中国水文数据**
   - 国家气象科学数据中心：http://data.cma.cn/

---

## 💻 本地实现方案

### 技术栈推荐
```
数据收集 → Python (requests, pandas)
   ↓
数据预处理 → Python (numpy, scipy)
   ↓
模型训练 → PyTorch/TensorFlow + LSTM/GRU/GNN
   ↓
预测可视化 → Streamlit/Dash
```

### 简单 Demo 结构
```
flood-prediction/
├── data/           # 降雨、水位数据
├── models/         # 训练好的模型
├── notebooks/      # Jupyter 实验笔记
├── src/
│   ├── data_loader.py
│   ├── model.py
│   ├── predict.py
│   └── visualize.py
└── README.md
```

---

## 📖 学习路径

### 入门 (1-2 周)
1. 学习 LSTM/GRU 时间序列预测
2. 了解基本水文学概念
3. 跑通一个开源 demo

### 进阶 (1 个月)
1. 研究 Google 论文细节
2. 尝试 GNN 建模河流网络
3. 整合卫星数据

### 实战 (2-3 个月)
1. 收集本地河流数据
2. 训练定制化模型
3. 部署预警系统

---

## 🚀 下一步行动

1. **克隆开源项目** - 选一个上面的项目先跑起来
2. **收集数据** - 确定你关心的河流/区域
3. **搭建环境** - Python + PyTorch + 相关库
4. **实验训练** - 从简单模型开始

---

*最后更新：2026-03-13*
*整理自 Google Research 及开源社区资源*
