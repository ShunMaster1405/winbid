# BidGenius - 智能投标文件生成系统

> AI 驱动的企业投标文件生成平台，通过大语言模型与企业知识库的结合，实现招标文件的自动解析、投标文件的智能生成与废标风险的主动识别。

## 功能特性

- **智能招标解析**：自动解析招标文件，提取评分项、废标项等关键信息
- **标书自动生成**：基于招标要求和知识库，智能生成投标文件
- **流式章节生成**：支持实时流式输出，提升交互体验
- **知识库管理**：企业文档向量化存储，语义检索复用历史内容
- **废标风险审查**：自动检查投标文件完整性与合规性
- **多模型支持**：兼容 OpenAI 格式的 LLM API，灵活配置

## 技术架构

```
Client (React + TypeScript)
        │
        ▼
  [API Gateway]  ← JWT 鉴权、限流、路由
        │
   ┌────┴─────────────────────────────┐
   │         微服务网络                │
   │                                  │
   ├── [Tender Parser]   :8001       │
   ├── [Bid Generator]   :8002       │
   ├── [Risk Checker]    :8003       │
   ├── [Knowledge Base]  :8004       │
   └── [Admin Service]   :8005       │
        │ 
   ┌────▼──────────────────────┐
   │      数据层                │
   │  PostgreSQL + pgvector      │
   │  Redis 7                  │
   │  MinIO                    │
   └───────────────────────────┘
```

## 技术栈

| 层级 | 技术 |
|------|------|
| 前端 | React + TypeScript + Vite + Ant Design X + Redux Toolkit |
| 后端 | Python 3.11 + FastAPI |
| 数据库 | PostgreSQL 15 + pgvector |
| 缓存 | Redis 7 |
| 对象存储 | MinIO |
| 认证 | JWT + bcrypt |
| 容器化 | Docker + docker-compose |

## 快速开始

### 前置要求

- Docker Desktop >= 4.0
- Node.js >= 18
- Python >= 3.11

### 1. 启动开发环境

```bash
# 复制环境变量模板
cp .env.example .env

# 编辑 .env 文件，填入实际配置
# 至少需要设置：JWT_SECRET、ENCRYPTION_KEY、DEFAULT_LLM_API_KEY

# 启动基础服务 (PostgreSQL, Redis, MinIO)
docker-compose up -d

# 检查服务状态
docker-compose ps
```

### 2. 初始化数据库

数据库会在容器启动时自动初始化。

默认管理员账号：
- 邮箱：`admin@bidgenius.com`
- 密码：`admin123`

⚠️ **生产环境请立即修改默认密码！**

### 3. 安装依赖

```bash
# 后端依赖
pip install -r libs/common/requirements.txt  # TODO: 创建 requirements.txt

# 前端依赖
cd frontend
npm install
```

### 4. 启动服务

```bash
# 启动前端
cd frontend
npm run dev

# 启动后端服务 (另开终端)
# 服务入口文件待创建
uvicorn services.admin.app.main:app --reload --port 8005
```

### 5. 访问应用

- 前端：http://localhost:5173
- API 文档：http://localhost:8000/docs
- MinIO 控制台：http://localhost:9001

## 项目结构

```
bidgenius/
├── docker-compose.yml          # 开发环境编排
├── .env.example               # 环境变量模板
├── infra/                     # 基础设施配置
│   └── postgres/               # 数据库初始化脚本
├── libs/                      # 共享库
│   ├── common/                # 通用工具
│   ├── auth/                  # 认证授权
│   ├── llm/                   # LLM 客户端
│   ├── document/               # 文档处理
│   └── vector/                # 向量检索
├── services/                  # 微服务
│   ├── api-gateway/           # API 网关
│   ├── tender-parser/         # 招标解析
│   ├── bid-generator/         # 标书生成
│   ├── risk-checker/          # 风险审查
│   ├── knowledge-base/        # 知识库
│   └── admin/                 # 系统管理
└── frontend/                  # React 前端
    └── src/
        ├── api/                # API 客户端
        ├── components/         # 组件
        ├── hooks/              # Hooks
        ├── pages/              # 页面
        └── stores/             # 状态管理
```

## 开发指南

### 添加新的共享库模块

在 `libs/` 下创建新目录，实现功能后更新 `__init__.py` 导出。

### 添加新的微服务

1. 在 `services/` 下创建服务目录
2. 实现 FastAPI 应用结构
3. 在 `docker-compose.yml` 中添加服务配置

### 添加新的前端页面

1. 在 `frontend/src/pages/` 下创建页面组件
2. 在路由配置中注册路由
3. 添加对应的 API 调用

## 环境变量说明

详见 `.env.example` 文件，关键配置：

| 变量 | 说明 |
|------|------|
| `JWT_SECRET` | JWT 签名密钥（至少 32 字符） |
| `ENCRYPTION_KEY` | AES-256 加密密钥（32 字节） |
| `DEFAULT_LLM_API_KEY` | 默认 LLM API Key |
| `POSTGRES_PASSWORD` | 数据库密码 |
| `MINIO_ROOT_PASSWORD` | MinIO 密码 |

## 安全说明

- 所有 API 请求需携带 JWT Token
- 数据按组织隔离，查询自动注入 `org_id`
- 文件上传有类型和大小限制
- API Key 加密存储

## 许可证

Copyright © 2026 BidGenius Team
# test-bid
# test-bid
# winbid
# winbid
