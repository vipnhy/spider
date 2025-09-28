# 数学题目爬取工具 (Mathematics Problem Scraper)

一个用于从在线教育平台获取数学题目的Python爬虫工具。

## 项目简介

本项目是一个专门用于爬取数学题目数据的工具，主要从17zuoye.com（一起作业网）获取小学数学题目内容。项目包含数据下载、处理和存储功能。

## 功能特性

- **多级数据获取**: 按年级、学期、教材版本获取题目
- **智能代理支持**: 内置代理池，支持IP轮换避免访问限制
- **数据结构化处理**: 自动整理题目内容、答案和分类信息
- **数据库存储**: 支持将爬取的数据存储到MySQL数据库
- **增量ID生成**: 内置ID生成器确保数据唯一性
- **容错机制**: 包含重试和错误处理机制

## 项目结构

```
spider/
├── main.py              # 主程序入口
├── downloader.py        # 核心下载模块
├── upload.py           # 数据库上传模块
├── para.py             # 参数配置文件
├── id_set.py           # ID生成器
├── test.py             # 测试脚本
├── zhixueips.txt       # 代理IP列表
└── README.md           # 项目说明文档
```

### 核心模块说明

#### 1. downloader.py
主要下载功能模块，包含以下核心函数：
- `get_book_list()`: 获取教材列表
- `get_keypoint_list()`: 获取知识点列表  
- `get_question_list()`: 获取题目ID列表
- `get_question_info()`: 获取详细题目信息
- `question_downloader()`: 主要下载流程控制

#### 2. upload.py
数据库操作模块，包含：
- 数据格式化处理
- MySQL数据库连接和操作
- 题目内容上传和存储

#### 3. para.py
配置参数文件，包含：
- 年级和学期设置
- Cookie和User-Agent配置
- 代理IP池配置

#### 4. id_set.py
唯一ID生成器，用于为数据库记录生成唯一标识符

## 安装配置

### 系统要求
- Python 3.6+
- MySQL 5.7+

### 依赖安装

```bash
pip install requests
pip install pymysql
pip install beautifulsoup4
```

### 配置步骤

1. **数据库配置**
   在`upload.py`中修改数据库连接信息：
   ```python
   db = pymysql.connect("127.0.0.1", "用户名", "密码", "数据库名")
   ```

2. **参数配置**
   在`para.py`中设置：
   ```python
   LEVEL = 3  # 年级 (1-6)
   TERM = 1   # 学期 (1-上册, 2-下册)
   COOKIE = 'your_cookie_here'  # 需要有效的登录Cookie
   ```

3. **代理配置**
   项目已包含代理IP列表文件`zhixueips.txt`，如需更新可替换该文件内容

## 使用方法

### 基本使用

1. **直接运行主程序**：
   ```bash
   python main.py
   ```

2. **使用下载器模块**：
   ```bash
   python downloader.py
   ```

3. **数据上传**：
   ```bash
   python upload.py
   ```

### 自定义配置

修改`para.py`中的参数来获取不同年级和学期的题目：

```python
LEVEL = 4  # 四年级
TERM = 2   # 下册
```

### 数据流程

1. **数据获取**: 从目标网站获取题目数据
2. **数据处理**: 清理和格式化题目内容
3. **本地存储**: 将数据保存为JSON文件
4. **数据库存储**: 上传到MySQL数据库

## 数据库结构

项目会创建以下数据表：

```sql
-- 题目表
CREATE TABLE questions (
    id CHAR(16) NOT NULL PRIMARY KEY,
    content TEXT,
    answer TEXT,
    grade_id INT,
    book_id CHAR(16),
    unit_id CHAR(16),
    section_id CHAR(16)
);

-- 其他相关表
CREATE TABLE books (...);
CREATE TABLE units (...);
CREATE TABLE sections (...);
```

## 注意事项

### 法律声明
- 本工具仅供学习和研究使用
- 请遵守目标网站的robots.txt和使用条款
- 不得将爬取的数据用于商业用途
- 使用时请注意访问频率，避免对目标服务器造成过大压力

### 技术注意
- Cookie可能会过期，需要定期更新
- 建议设置适当的请求间隔避免被封禁
- 代理IP质量会影响爬取效果
- 数据库配置需要提前准备

### 错误处理
项目包含完善的错误处理机制：
- 网络超时自动重试
- 代理IP自动切换
- 异常情况日志记录

## 开发贡献

欢迎提交Issue和Pull Request来改进项目。

### 开发环境设置
1. Fork项目到你的GitHub
2. 创建新的功能分支
3. 提交你的修改
4. 创建Pull Request

## 版本历史

- v1.0: 基础爬虫功能
- 持续更新中...

## 许可证

本项目基于MIT许可证，详见LICENSE文件。

## 联系方式

如有问题或建议，请通过以下方式联系：
- GitHub Issues
- 项目维护者

---

**免责声明**: 本工具仅用于教育和研究目的，使用者需自行承担使用风险并遵守相关法律法规。