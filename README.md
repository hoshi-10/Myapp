# Myapp
软件工程大作业
# 智能报价管理系统

> 基于 Vue 3 + Element Plus + Node.js + Express + MySQL + Python + LangChain 的智能报价管理系统

## 项目简介

智能报价管理系统是一套面向报价业务场景设计的 Web 管理系统，主要用于解决传统 Excel 报价文件不易管理、历史报价难以查询以及报价数据分析效率较低等问题。

系统支持用户上传 Excel 报价文件，自动解析其中的商品及价格信息，并将报价数据结构化保存到数据库中。用户可以查看当前报价、管理历史报价记录，并根据需要打印报价单。

在基础报价管理功能之外，系统引入 Python + LangChain 构建 AI 报价分析 Agent。Agent 可以根据用户提出的问题，调用不同的报价数据工具，对当前报价及历史数据进行查询和分析，并生成自然语言形式的分析结果。

---

## 主要功能

### 用户管理

* 用户注册与登录
* JWT 身份认证
* 用户信息管理
* 用户报价数据隔离

### 报价管理

* Excel 报价文件上传
* Excel 数据自动解析
* 报价商品信息展示
* 报价总金额计算
* 报价数据保存
* 报价信息查看、修改与删除
* 历史报价记录查询
* 报价单打印

### 报价数据分析

* 报价单数量统计
* 报价金额统计
* 报价数据汇总
* 历史报价数据查询

### AI 报价分析助手

系统使用 Python + LangChain 构建报价分析 Agent。

Agent 可以根据用户的问题选择合适的数据工具，例如：

* 查询当前报价单
* 查询报价商品明细
* 查询历史报价
* 计算报价统计数据

用户可以向 AI 提出类似的问题：

> “帮我总结一下这份报价单。”

> “这份报价一共有多少钱？”

> “哪些商品的价格比较高？”

> “和历史报价相比有什么变化？”

Agent 根据实际报价数据进行分析，并返回自然语言结果。

---

## 技术栈

### 前端

| 技术           | 用途          |
| ------------ | ----------- |
| Vue 3        | 前端页面开发      |
| Vite         | 前端构建工具      |
| Element Plus | UI 组件库      |
| Vue Router   | 页面路由        |
| Axios        | 前后端 HTTP 通信 |

### 后端

| 技术      | 用途         |
| ------- | ---------- |
| Node.js | 后端运行环境     |
| Express | Web 服务框架   |
| JWT     | 用户身份认证     |
| MySQL   | 数据持久化      |
| xlsx    | Excel 文件解析 |

### AI 服务

| 技术        | 用途             |
| --------- | -------------- |
| Python    | AI 服务开发        |
| LangChain | Agent 开发框架     |
| LLM       | 自然语言理解与生成      |
| FastAPI   | Python AI 服务接口 |

---

## 系统架构

系统采用前后端分离架构，并将 AI Agent 作为独立服务运行。

```text
                    ┌─────────────────────┐
                    │      Vue 3 前端      │
                    │    Element Plus     │
                    └──────────┬──────────┘
                               │
                             Axios
                               │
                               ↓
                    ┌─────────────────────┐
                    │  Node.js + Express  │
                    │                     │
                    │ 用户管理             │
                    │ 报价管理             │
                    │ Excel解析            │
                    │ JWT认证              │
                    └───────┬───────┬─────┘
                            │       │
                            ↓       ↓
                       ┌───────┐  ┌─────────────────┐
                       │ MySQL │  │ Python AI 服务   │
                       │       │  │                 │
                       └───────┘  │ LangChain       │
                                  │ Agent           │
                                  │                 │
                                  │ ┌─────────────┐ │
                                  │ │报价查询工具 │ │
                                  │ ├─────────────┤ │
                                  │ │历史查询工具 │ │
                                  │ ├─────────────┤ │
                                  │ │数据统计工具 │ │
                                  │ └─────────────┘ │
                                  └─────────────────┘
```

---

## 系统业务流程

### 1. 用户登录

```text
用户
 ↓
输入用户名和密码
 ↓
Node.js 后端验证
 ↓
生成 JWT
 ↓
前端保存 Token
 ↓
进入系统
```

### 2. 上传报价单

```text
用户上传 Excel
 ↓
Vue 前端发送文件
 ↓
Node.js 接收文件
 ↓
xlsx 解析 Excel
 ↓
提取报价数据
 ↓
计算报价金额
 ↓
保存到 MySQL
 ↓
返回报价数据
 ↓
Vue 展示报价单
```

### 3. 查看历史报价

```text
用户进入历史报价
 ↓
Vue 请求 Node.js API
 ↓
Node.js 查询 MySQL
 ↓
返回当前用户的报价记录
 ↓
前端展示
```

### 4. AI 报价分析

```text
用户提出问题
 ↓
Vue
 ↓
Node.js
 ↓
Python AI 服务
 ↓
LangChain Agent
 ↓
判断需要使用的工具
 ↓
查询报价数据
 ↓
LLM 分析数据
 ↓
返回分析结果
 ↓
Node.js
 ↓
Vue 展示
```

---

## 数据库设计

系统主要包含以下数据表：

### users

用于保存系统用户信息。

```text
id
username
password
role
created_at
```

### quotes

用于保存报价单基本信息。

```text
id
user_id
title
customer_name
total_amount
file_name
created_at
updated_at
```

### quote_items

用于保存报价单中的商品明细。

```text
id
quote_id
product_name
specification
quantity
unit_price
amount
```

数据关系：

```text
users
  │
  │ 1 : N
  ↓
quotes
  │
  │ 1 : N
  ↓
quote_items
```

---

## 项目结构

项目采用前后端分离结构：

```text
smart-quotation-system/
│
├── frontend/                  # Vue 3 前端
│   ├── src/
│   │   ├── views/             # 页面
│   │   ├── components/        # 公共组件
│   │   ├── router/            # 路由
│   │   ├── api/               # API 请求
│   │   └── stores/             # 状态管理
│   └── package.json
│
├── backend/                   # Node.js 后端
│   ├── routes/                # 路由
│   ├── controllers/           # 控制器
│   ├── services/              # 业务逻辑
│   ├── middleware/            # 中间件
│   ├── db/                    # 数据库连接
│   ├── uploads/               # 上传文件
│   ├── index.js               # 服务入口
│   └── package.json
│
├── ai-service/                # Python AI 服务
│   ├── agent/                 # LangChain Agent
│   ├── tools/                 # Agent 工具
│   ├── routes/                # AI接口
│   ├── main.py                # 服务入口
│   └── requirements.txt
│
├── database/                  # 数据库脚本
│   └── init.sql
│
└── README.md
```

---

## 项目运行

### 一、运行前端

进入前端目录：

```bash
cd frontend
```

安装依赖：

```bash
npm install
```

启动开发服务器：

```bash
npm run dev
```

---

### 二、运行 Node.js 后端

进入后端目录：

```bash
cd backend
```

安装依赖：

```bash
npm install
```

配置数据库连接信息后启动：

```bash
npm start
```

或者：

```bash
node index.js
```

默认后端地址：

```text
http://localhost:3000
```

---

### 三、运行 Python AI 服务

进入 AI 服务目录：

```bash
cd ai-service
```

创建虚拟环境：

```bash
python -m venv .venv
```

激活虚拟环境：

Windows：

```bash
.venv\Scripts\activate
```

Linux / macOS：

```bash
source .venv/bin/activate
```

安装依赖：

```bash
pip install -r requirements.txt
```

启动 AI 服务：

```bash
python main.py
```

---

## 环境变量

项目中的数据库密码、JWT 密钥以及 AI API Key 不直接写入源代码。

Node.js 后端可以使用：

```text
.env
```

例如：

```env
PORT=3000

DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=smart_quotation

JWT_SECRET=your_jwt_secret

AI_SERVICE_URL=http://localhost:8000
```

Python AI 服务同样建议通过环境变量配置模型相关信息。

> `.env` 文件不应提交到 GitHub。

---

## AI Agent 设计

AI 模块并不是简单地将整份报价单直接发送给大语言模型，而是通过 LangChain 构建 Agent，并为 Agent 提供报价数据相关工具。

### Agent 工具

```text
get_quote()
```

查询当前报价单。

```text
get_quote_items()
```

查询报价商品明细。

```text
get_quote_history()
```

查询历史报价数据。

```text
calculate_quote_statistics()
```

计算报价数量、金额等统计数据。

Agent 根据用户问题选择合适的工具获取数据，再结合大语言模型生成分析结果。

---

## 项目定位

本项目主要用于学习和实践以下技术：

* Vue 3 前端开发
* Element Plus UI 开发
* Node.js / Express 后端开发
* MySQL 数据库设计
* Excel 文件解析
* RESTful API 设计
* JWT 身份认证
* Python AI 服务开发
* LangChain Agent
* 前后端分离架构
* AI 与传统业务系统结合
