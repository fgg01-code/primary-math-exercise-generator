# stock-data-visual-lab
# 金融股票数据采集清洗与可视化分析

> 基于 Python 的 A 股 2025 年全景式数据分析 Pipeline

---

## 项目简介

本项目基于《金融股票数据采集清洗与可视化分析》结课论文的技术方案，实现了一套完整的金融大数据处理工作流。以 **2025 年 A 股市场**为实证对象，覆盖从原始行情数据采集到高质量可视化分析输出的全流程。

### 核心功能

| 模块 | 功能 | 技术方案 |
|------|------|----------|
| **数据采集** | 批量获取日线数据 | AKShare + Tushare 双源架构 |
| **数据清洗** | 三阶段质量治理 | 缺失值填充 → 3σ 法则 → Z-score 标准化 |
| **数据分析** | 统计与相关性分析 | 描述性统计、Pearson 相关系数矩阵 |
| **可视化** | 5 张专业图表 | Matplotlib, 200 DPI |

### 实证对象

- **四大指数**: 上证指数、沪深 300、创业板指、科创 50
- **四只个股**: 贵州茅台(600519)、宁德时代(300750)、比亚迪(002594)、招商银行(600036)
- **时间范围**: 2025-01-02 ~ 2025-12-31（242 个交易日）

---

## 目录结构

```
金融股票数据采集清洗与可视化分析/
├── README.md               # 项目说明与使用指南
├── requirements.txt        # Python 依赖清单
├── config.py               # 全局配置文件
├── main.py                 # 主运行入口（支持分步执行）
├── data_collector.py       # 数据采集模块
├── data_cleaner.py         # 数据清洗模块
├── analyzer.py             # 数据分析模块
├── visualizer.py           # 可视化模块
├── data/
│   ├── raw/                # 原始采集数据 (CSV)
│   └── clean/              # 清洗后数据 (CSV)
└── output/                 # 分析结果与图表 (PNG/CSV)
```

---

## 快速开始

### 1. 环境要求

- Python 3.8+
- Windows / macOS / Linux

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

### 3. 运行方式

#### 完整流程（推荐）

```bash
python main.py
```

#### 分步运行

```bash
python main.py --step collect      # 仅数据采集
python main.py --step clean        # 仅数据清洗
python main.py --step analyze      # 仅数据分析
python main.py --step visualize    # 仅可视化
```

#### 单独运行各模块

```bash
python data_collector.py           # 采集原始数据
python data_cleaner.py             # 清洗原始数据
python analyzer.py                 # 分析清洗后数据
python visualizer.py               # 生成图表
```

### 4. 配置说明

编辑 `config.py` 修改以下关键参数：

```python
# 时间范围
START_DATE = "20250101"
END_DATE   = "20251231"

# 3σ 法则阈值
SIGMA_THRESHOLD = 3.0

# Tushare Token（可选，用于双源备份）
TUSHARE_TOKEN = "your_token_here"
```

---

## 技术方案详解

### 数据采集（data_collector.py）

```
AKShare (主) ──→ stock_zh_a_hist() ──→ OHLCV 数据
     │                                      │
     ├── 失败回退 ──→  Tushare Pro  ────────┘
     │
     └── 请求间隔 1s ──→ 断点重试 ──→ CSV 本地持久化
```

- 前复权方式处理分红送转影响
- 每条记录包含开盘价、最高价、最低价、收盘价、成交量、成交额
- 单只股票独立 CSV + 汇总文件双重存储

### 数据清洗（data_cleaner.py）

```
阶段1: 缺失值处理
  ├── 缺失率统计与分析
  ├── 前向填充 (Forward Fill)：用最近有效值替代
  └── 后向填充 (Backward Fill) 兜底首行

阶段2: 3σ 法则异常值检测
  ├── 基于日收益率计算 μ 和 σ
  ├── 标记超出 [μ-3σ, μ+3σ] 的异常值
  └── Winsorize 缩尾处理（裁剪而非删除）

阶段3: Z-score 标准化
  └── z = (r - μ') / σ'  → 均值 0，标准差 1
```

### 可视化图表（visualizer.py）

| 图号 | 图表名称 | 类型 | 核心发现 |
|------|----------|------|----------|
| 3.1 | 指数年度走势 | 折线图 | 前稳后升，科技成长大幅领跑 |
| 3.2 | 贵州茅台日K线 | K线+成交量 | 下半年突破上行，均线多头排列 |
| 3.3 | 收益率分布直方图 | 直方图+正态拟合 | "尖峰肥尾"特征，3σ 法则有效 |
| 3.4 | 相关性热力图 | 热力图 | 板块内高相关，跨板块低相关 |
| 3.5 | 年度涨跌幅对比 | 柱状图 | 创业板指 +49.57% 领跑全市场 |

---

## 实验结果摘要

基于 2025 年真实市场数据（模拟）：

| 指标 | 创业板指 | 科创50 | 上证指数 | 沪深300 |
|------|----------|--------|----------|---------|
| 年度涨跌幅 | **+49.57%** | +35.92% | +18.41% | +17.66% |
| 风格 | 科技成长 | 科技成长 | 大盘价值 | 大盘价值 |

**核心结论**: 2025 年 A 股呈现显著的结构性牛市，科技成长风格占据主导地位，验证了数据驱动分析框架在金融研究中的实用价值。

---

## 依赖说明

| 包名 | 版本 | 用途 |
|------|------|------|
| pandas | >=2.0 | 数据处理核心 |
| numpy | >=1.24 | 数值计算 |
| akshare | >=1.12 | 金融数据接口（主） |
| tushare | >=1.4 | 金融数据接口（备） |
| matplotlib | >=3.7 | 可视化绘图 |
| scipy | >=1.11 | 科学计算（正态拟合等） |

---

## 参考文献

[1] McKinney W. Data Structures for Statistical Computing in Python. 2020.
[2] 梁谦刚. 暴涨1820%，2025年度牛股出炉！证券时报网, 2025.
[3] 中金公司研究部. 2025年A股复盘：重山已过，乘势笃行. 2026.
[4] Box G E P, et al. Time Series Analysis: Forecasting and Control (5th). 2025.
[5] Hunter J D. Matplotlib: A 2D Graphics Environment. 2017.

---

## 许可

本项目仅用于学术研究与课程教学目的。
