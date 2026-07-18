# stock-data-visual-lab
primary-math-exercise-generator
小学生数学练习题目自动生成系统
项目简介
本系统基于 Python 开发，面向 1-6 年级小学生自动生成数学练习题，支持加减乘除、四则混合、带括号运算、基础应用题等题型。可自定义题目数量、数值区间、是否进位、有无余数等难度参数，内置答案自动计算功能，支持导出文本与 Word 试卷，便于直接打印。项目采用模块化设计，配置简单、拓展性强，能够大幅减少教师、家长手动出题的时间成本，适用于课后作业、随堂小测、家庭日常数学巩固练习。
项目结构
plaintext
primary-math-exercise-generator/
├── config.py          # 年级、题型、数值范围配置
├── generator.py       # 题目生成核心逻辑
├── answer_calc.py     # 标准答案自动计算模块
├── exporter.py        # 文本/Word文档导出工具
├── main.py            # 程序运行入口
├── requirements.txt   # 依赖库清单
└── output/            # 生成的试卷与答案文件
环境与运行
安装依赖
bash
运行
pip install -r requirements.txt
一键生成练习卷
bash
运行
python main.py
自定义参数
修改config.py可切换年级、运算类型、题目数量、数值上限。
核心功能
多题型生成：纯加减、乘除、四则混合、括号运算、简单应用题
难度可控：自定义数字范围、开关进位、余数、小数
自动配套标准答案，分开导出方便批改
支持 txt、docx 两种文件格式输出
模块化低耦合，可新增题型、拓展高年级功能
适用场景
小学教师批量布置课堂作业、家长居家辅导自测、培训机构随堂测试出题。
