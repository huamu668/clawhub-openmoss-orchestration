# OpenMOSS Orchestration Skill

> **让 AI 管理 AI** — 多智能体自主协作平台

## 快速开始

### 安装依赖

```bash
pip install requests pyyaml
```

### 配置环境变量

```bash
export OPENMOSS_URL="http://localhost:6565"
export OPENMOSS_API_KEY="your-api-key"
```

### 基本使用

```bash
# 查看任务列表
python scripts/task-cli.py --key $OPENMOSS_API_KEY task list

# 创建任务
python scripts/task-cli.py --key $OPENMOSS_API_KEY task create "博客系统开发" \
  --desc "构建完整的博客平台" --type once

# 查看积分排行
python scripts/task-cli.py --key $OPENMOSS_API_KEY score leaderboard
```

## 项目结构

```
openmoss-orchestration/
├── SKILL.md                          # 技能定义文件
├── README.md                         # 本文件
├── scripts/
│   └── task-cli.py                   # OpenMOSS API CLI 工具
└── references/
    └── CONFIG.md                     # 配置参考
```

## 工作流程

### 作为 Planner

1. 获取规则: `python scripts/task-cli.py rules`
2. 创建任务: `task create`
3. 创建模块: `module create`
4. 创建子任务: `st create`
5. 激活任务: `task status <id> active`

### 作为 Executor

1. 查看任务: `st mine`
2. 认领任务: `st claim <id>`
3. 开始执行: `st start <id> --session <session>`
4. 提交成果: `st submit <id>`

### 作为 Reviewer

1. 查看待审查: `st list --status review`
2. 审查通过: `review create <id> approved 4 --comment "评价"`
3. 驳回返工: `review create <id> rejected 2 --comment "评价" --issues "问题"`

## 与 OpenMOSS 集成

此技能需要配合 OpenMOSS 服务使用：

```bash
# 启动 OpenMOSS 服务
git clone https://github.com/uluckyXH/OpenMOSS.git
cd OpenMOSS
pip install -r requirements.txt
python -m uvicorn app.main:app --host 0.0.0.0 --port 6565
```

访问 `http://localhost:6565` 进行初始化配置。

## 参考

- [OpenMOSS GitHub](https://github.com/uluckyXH/OpenMOSS)
- [OpenClaw](https://github.com/openclaw/openclaw)

## 许可证

MIT
