# 饥荒服务器管理平台（DST Server Management Platform）

对标开源项目 [DMP](https://github.com/dyphire/dmp) 的《饥荒：联机版》（Don't Starve Together）专用服务器管理平台，提供集群生命周期管理、世界与 Mod 配置、玩家名单、控制台命令、日志检索与运行监控的一站式 Web 管理界面。

## 功能

| 模块 | 能力 |
|---|---|
| 登录鉴权 | JWT 登录，管理员 / 普通用户角色区分 |
| 仪表盘 | 全局状态总览 |
| 集群管理 | 集群创建、启动 / 停止 / 重启、运行状态与进程状态展示（支持 Master / Caves 分服） |
| 世界配置 | 世界大小、季节、游戏模式、人数上限、洞穴开关等可视化编辑并保存应用 |
| Mod 管理 | Mod 列表、启用 / 禁用、参数配置、优先级排序 |
| 玩家管理 | 在线 / 全部玩家、管理员（adminlist）、白名单（whitelist）、封禁名单（blocklist） |
| 控制台 | 控制台命令执行与快捷命令入口 |
| 日志 | 服务器日志查看，按级别过滤、关键词搜索 |
| 监控 | 在线人数、内存等指标趋势图 |
| 系统设置 | Steam 令牌、基础参数、数据备份等 |

## 技术栈

- **后端**：NestJS 10、TypeScript、Drizzle ORM、PostgreSQL
- **前端**：React 19、Vite、TypeScript、Tailwind CSS、shadcn/ui（Radix UI）、ECharts
- **工程化**：npm（Node >= 22）、ESLint、Prettier、Stylelint、Jest

## 快速开始

环境要求：Node.js 22+、npm 10+、PostgreSQL。

### 1. 安装依赖

```bash
npm install
```

### 2. 启动开发环境

```bash
npm run dev        # 同时启动后端与前端
```

或分开启动：

```bash
npm run dev:server # 后端（NestJS watch，默认端口 3000，可用 SERVER_PORT 覆盖）
npm run dev:client # 前端（Vite dev server）
```

### 3. 构建与生产运行

```bash
npm run build      # 构建前端产物 + 后端编译
npm start          # 生产模式启动（默认 localhost:3000）
```

## 数据表

数据库 schema 位于 `server/database/schema.ts`（Drizzle ORM 自动生成），主要业务表：

| 表 | 用途 |
|---|---|
| dst_users | 用户 |
| dst_clusters / dst_servers | 集群与服务器 |
| dst_world_config | 世界配置 |
| dst_mods | Mod 配置 |
| dst_players / dst_player_lists | 玩家与名单 |
| dst_console_commands | 控制台命令记录 |
| dst_server_logs | 服务器日志 |
| dst_monitoring_stats | 监控指标 |
| dst_system_settings | 系统设置 |

## 目录结构

```text
.
├── client/                  # React 前端
│   └── src/pages/           # 页面：Dashboard/Clusters/WorldConfig/Mods/Players/Console/Logs/Monitoring/Settings/Login
├── server/                  # NestJS 后端
│   ├── database/schema.ts   # Drizzle ORM schema
│   └── modules/             # auth/dashboard/clusters/world-config/mods/players/console/logs/monitoring/settings
├── shared/                  # 前后端共享类型
├── scripts/                 # 开发与构建脚本
└── .env                     # 日志等环境配置
```

## 说明

- 服务器启停、日志生成、控制台命令执行等涉及真实 DST 游戏进程的能力，当前为平台内模拟实现；对接真实服务器进程需在服务层接入对应的进程管理实现。
- 默认端口：后端 3000（`SERVER_PORT`），前端由 Vite 分配。
