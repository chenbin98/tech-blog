# 洪水预测入门 Demo

## 快速开始

### 1. 环境准备
```bash
cd /Users/binchen/.openclaw/workspace/flood-prediction
python3 -m venv venv
source venv/bin/activate
pip install pandas numpy scikit-learn torch matplotlib
```

### 2. 项目结构
```
flood-prediction/
├── data/              # 存放降雨、水位数据
├── models/            # 存放训练好的模型
├── src/
│   ├── train.py      # 训练脚本
│   └── predict.py    # 预测脚本
└── README.md
```

### 3. 核心代码示例

#### 数据加载 (src/data_loader.py)
```python
import pandas as pd
import numpy as np

def load_rainfall_data(filepath):
    """加载降雨数据"""
    df = pd.read_csv(filepath, parse_dates=['date'])
    return df

def create_sequences(data, seq_length=7):
    """创建时间序列样本"""
    xs, ys = [], []
    for i in range(len(data) - seq_length):
        x = data[i:i+seq_length]
        y = data[i+seq_length]
        xs.append(x)
        ys.append(y)
    return np.array(xs), np.array(ys)
```

#### LSTM 模型 (src/model.py)
```python
import torch
import torch.nn as nn

class FloodPredictor(nn.Module):
    def __init__(self, input_size=1, hidden_size=50, num_layers=2):
        super().__init__()
        self.lstm = nn.LSTM(input_size, hidden_size, num_layers, batch_first=True)
        self.fc = nn.Linear(hidden_size, 1)
    
    def forward(self, x):
        out, _ = self.lstm(x)
        out = self.fc(out[:, -1, :])
        return out
```

#### 训练脚本 (src/train.py)
```python
import torch
import numpy as np
from model import FloodPredictor

# 准备数据
rainfall = np.random.randn(1000, 1)  # 示例数据
seq_length = 7
X, y = [], []
for i in range(len(rainfall) - seq_length):
    X.append(rainfall[i:i+seq_length])
    y.append(rainfall[i+seq_length])

X = torch.FloatTensor(X)
y = torch.FloatTensor(y)

# 训练模型
model = FloodPredictor()
criterion = nn.MSELoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)

for epoch in range(100):
    optimizer.zero_grad()
    output = model(X)
    loss = criterion(output, y)
    loss.backward()
    optimizer.step()
    if epoch % 10 == 0:
        print(f'Epoch {epoch}, Loss: {loss.item():.4f}')

# 保存模型
torch.save(model.state_dict(), '../models/flood_model.pth')
print('模型已保存!')
```

### 4. 运行训练
```bash
cd src
python train.py
```

---

## 📊 数据来源建议

### 中国地区
- **中国气象数据网**: http://data.cma.cn/
- **国家水文数据库**: 需申请访问

### 全球数据
- **GloFAS**: 欧洲全球洪水预警系统
- **NASA GPM**: 全球降雨测量卫星数据

---

## 🔧 下一步

1. 替换示例数据为真实降雨/水位数据
2. 调整模型参数优化预测精度
3. 添加可视化界面
4. 集成到 OpenClaw 定时任务

---

*创建时间：2026-03-13*
