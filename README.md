# 宽带资费对比

三大运营商宽带套餐资费对比平台，苹果官网风格的全栈项目。

## 项目简介

一站式对比中国移动、中国联通、中国电信的宽带套餐资费。涵盖 200M / 500M / 1000M 主流带宽档位，提供月费、合约期、特色服务等多维度对比，帮助你快速选出最合适的宽带套餐。

## 技术栈

| 层级 | 技术 |
|------|------|
| 前端框架 | React 18 |
| 构建工具 | Vite |
| 样式方案 | Tailwind CSS (苹果风格主题) |
| 动画 | Framer Motion |
| 后端框架 | Express.js |
| 数据存储 | JSON 文件 |

## 项目结构

```
broadband-compare/
├── client/                      # 前端项目
│   ├── src/
│   │   ├── components/          # React 组件
│   │   │   ├── Navbar.jsx       # 粘性导航栏（毛玻璃效果）
│   │   │   ├── Hero.jsx         # 全屏 Hero 区域（深色渐变背景）
│   │   │   ├── OperatorSection.jsx  # 运营商套餐展示区块
│   │   │   ├── PlanCard.jsx     # 套餐卡片
│   │   │   ├── ComparisonTable.jsx  # 横向对比表格
│   │   │   ├── ScrollReveal.jsx # 滚动渐入动画包装组件
│   │   │   └── Footer.jsx       # 页脚
│   │   ├── App.jsx              # 页面组装入口
│   │   ├── main.jsx             # React 挂载入口
│   │   └── index.css            # Tailwind 导入 + 全局样式
│   ├── index.html
│   ├── vite.config.js           # Vite 配置（含 API 代理）
│   └── package.json
├── server/                      # 后端项目
│   ├── index.js                 # Express 服务入口（端口 3001）
│   ├── routes/
│   │   └── plans.js             # 套餐 API 路由
│   └── data/
│       └── plans.json           # 套餐数据（3 运营商 × 3 档位）
└── package.json                 # 根配置（concurrently 一键启动）
```

## API 接口

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/plans` | 获取全部套餐，支持 `?operator=中国移动` 筛选 |
| GET | `/api/plans/:id` | 获取单个套餐详情 |
| GET | `/api/operators` | 获取运营商列表及对应套餐 |

## 本地启动

### 环境要求

- Node.js >= 18

### 安装依赖

```bash
# 根目录（concurrently）
npm install

# 前端
cd client && npm install

# 后端
cd ../server && npm install
```

### 启动方式一：分别启动

**启动后端（端口 3001）：**

```bash
cd server
node index.js
```

**启动前端（端口 5173）：**

```bash
cd client
npm run dev
```

然后访问 `http://localhost:5173`。

> 前端通过 Vite 代理将 `/api` 请求转发到后端 `http://localhost:3001`，开发时无需额外配置。

### 启动方式二：一键启动

```bash
# 在项目根目录下
npm run dev
```

该命令使用 concurrently 同时启动前后端，适合日常开发。

---

宽带对比 © 2025
