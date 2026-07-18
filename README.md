# 小学生数学练习题目自动生成系统

一键生成 1-6 年级数学练习卷，支持 TXT 和 DOCX（Word 试卷排版）两种导出格式。

## 功能特性

- **6 个年级预设**：覆盖小学全阶段，数值范围、题型、难度随年级递增
- **6 种题型**：加法、减法、乘法、除法、四则混合运算（可带括号）、简单应用题
- **灵活配置**：进位/退位、带余数除法、小数运算等开关自由组合
- **自动答案**：内置独立答案计算与校验模块，确保准确
- **双格式导出**：
  - TXT：题目和答案分文件输出，便于打印
  - DOCX：Word 试卷排版，A4 纸型，可直接打印使用

## 项目结构

```
primary-math-exercise-generator/
├── config.py          # 难度参数配置（1-6年级预设）
├── generator.py       # 题目生成核心逻辑
├── answer_calc.py     # 标准答案计算与校验
├── exporter.py        # TXT / DOCX 导出工具
├── main.py            # 程序入口（命令行）
├── requirements.txt   # 依赖清单
└── README.md          # 项目说明
```

## 快速开始

### 1. 安装依赖

```bash
pip install -r requirements.txt
```

### 2. 一键生成

```bash
# 默认：一年级 20 题，输出到桌面 primary-math-output/
python main.py

# 三年级 30 题
python main.py -g 3 -n 30

# 五年级 50 题，输出到指定目录
python main.py -g 5 -n 50 -o D:/练习卷

# 固定随机种子，可复现同一套题
python main.py -g 2 -n 40 --seed 42

# 查看所有年级预设配置
python main.py --list-grades
```

### 3. 输出文件

运行后在输出目录下生成：

| 文件 | 说明 |
|------|------|
| `{年级}_练习_题目.txt` | 纯题目，学生答题用 |
| `{年级}_练习_答案.txt` | 题目 + 答案对照 |
| `{年级}_练习.docx` | Word 试卷，A4 排版，含答案页 |

## 进阶用法

### 在代码中调用

```python
from config import get_grade_config
from generator import ExerciseGenerator
from answer_calc import verify_answers
from exporter import export_all

# 生成题目
cfg = get_grade_config(3)           # 三年级预设
gen = ExerciseGenerator(cfg, seed=42)
questions = gen.generate(30)

# 校验答案
report = verify_answers(questions)
print(f"正确率: {report['correct']}/{report['total']}")

# 导出
files = export_all(questions, cfg.grade_name, "./output")
```

### 自定义难度配置

```python
from config import create_custom_config
from generator import ExerciseGenerator

cfg = create_custom_config(
    grade=99,                        # 自定义标识
    grade_name="自定义练习",
    min_num=10,
    max_num=100,
    enable_addition=True,
    enable_subtraction=True,
    enable_multiplication=True,
    enable_division=True,
    enable_mixed=True,
    allow_carry=True,
    allow_borrow=True,
    allow_remainder=True,
    allow_parentheses=True,
    decimal_places=1,                # 一位小数
)

gen = ExerciseGenerator(cfg)
questions = gen.generate(50)
```

## 各年级配置概览

| 年级 | 数值范围 | 启用题型 | 特色 |
|------|----------|----------|------|
| 一年级 | 0~20 | 加减法 | 无进位/退位 |
| 二年级 | 0~100 | 加减法、乘法 | 进位/退位、九九乘法表 |
| 三年级 | 0~1000 | 加减乘除、四则混合 | 括号选项 |
| 四年级 | 0~10000 | 加减乘除、四则混合 | 带余数除法、扩大的乘法表 |
| 五年级 | 0~100000 | 全题型 + 应用题 | 一位小数 |
| 六年级 | 0~1000000 | 全题型 + 应用题 | 两位小数、更大数值 |

## 许可证

MIT License
