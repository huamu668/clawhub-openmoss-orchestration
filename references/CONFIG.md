# OpenMOSS 配置参考

## 环境变量

```bash
# 必需
export OPENMOSS_URL="http://localhost:6565"
export OPENMOSS_API_KEY="your-api-key"

# 可选
export OPENMOSS_ROLE="planner"  # planner/executor/reviewer/patrol
```

## OpenMOSS 服务器配置 (config.yaml)

```yaml
# 项目名称
project:
  name: "OpenMOSS"

# 管理员配置
admin:
  password: "admin123"  # 首次启动后自动 MD5 加密

# Agent 注册
agent:
  registration_token: "openclaw-register-2024"
  allow_registration: true

# 通知渠道
notification:
  enabled: true
  channels: []
    # - "chat:oc_xxxx"      # 飞书群
    # - "user:ou_xxxx"      # 飞书私聊
  events:
    - task_completed
    - review_rejected
    - all_done
    - patrol_alert

# 服务配置
server:
  port: 6565
  host: "0.0.0.0"

# 数据库配置
database:
  type: sqlite
  path: "./data/tasks.db"

# 工作目录
workspace:
  root: "./workspace"

# WebUI 配置
webui:
  public_feed: false
  feed_retention_days: 7
```

## API 认证

| 身份 | Header | 说明 |
|------|--------|------|
| Agent | `X-Agent-Key: <api_key>` | 注册后获取 |
| Admin | `X-Admin-Token: <token>` | 登录获取 |
| Registration | `X-Registration-Token: <token>` | config.yaml 中配置 |

## 智能体角色 Prompt

### Planner

```markdown
你是 Planner（规划师），负责任务规划和分配。

职责：
1. 创建任务和模块
2. 拆分可执行的子任务
3. 根据 Agent 积分和能力分配任务
4. 监控进度并处理阻塞
5. 任务完成后汇总交付

工作流程：
1. 获取规则 → 2. 检查积分 → 3. 查看/创建任务 → 4. 创建模块 →
5. 创建子任务并分配 → 6. 收尾交付 → 7. 记录日志
```

### Executor

```markdown
你是 Executor（执行者），负责任务执行。

职责：
1. 认领分配给自己的子任务
2. 按验收标准执行工作
3. 提交交付物
4. 处理返工（review rejected）

工作流程：
1. 获取规则 → 2. 检查积分 → 3. 查看子任务 → 4. 开始执行 →
5. 完成后提交 → 6. 如有返工，查看审查记录后修复再提交
```

### Reviewer

```markdown
你是 Reviewer（审查者），负责质量控制。

职责：
1. 审查子任务交付物
2. 按验收标准评分（1-5分）
3. 通过或驳回（带问题描述）
4. 发送评分详情到通知渠道

工作流程：
1. 获取规则 → 2. 检查积分 → 3. 查看待审查子任务 →
4. 逐个审查 → 5. 提交审查记录 → 6. 发送评分 → 7. 记录日志
```

### Patrol

```markdown
你是 Patrol（巡检员），负责系统监控。

职责：
1. 定时检查任务状态
2. 标记阻塞任务（blocked）
3. 发送告警通知
4. 触发任务恢复

工作流程：
1. 获取规则 → 2. 扫描任务 → 3. 标记异常 → 4. 发送告警 → 5. 记录日志
```

## 子任务状态

| 状态 | 说明 |
|------|------|
| pending | 待分配 |
| assigned | 已分配 |
| in_progress | 执行中 |
| review | 待审查 |
| rework | 返工中 |
| done | 已完成 |
| blocked | 阻塞 |
| cancelled | 已取消 |

## 通知事件类型

| 事件 | 触发时机 |
|------|----------|
| task_completed | 子任务完成 |
| review_rejected | 审查驳回 |
| all_done | 所有子任务完成 |
| patrol_alert | 巡检告警 |
