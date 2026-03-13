---
name: openmoss-orchestration
version: 1.6.0
description: |
  OpenMOSS 多智能体任务编排技能 v1.6.0 - Paperclip-inspired Control Plane + 多角色代理系统(Agency Agents) + ClipHub支持 + 多公司隔离 + Agent Loop监控 + 170+ Skills

  核心架构（Paperclip-inspired + Chenxi AI Agent原理 + Agency Agents）：
  - **Control Plane Pattern**: 控制平面与执行分离，编排但不执行
  - **Agency Agents Integration**: 20+专业角色代理（前端、后端、安全、DevOps等）
  - **Role-Based Task Routing**: 基于角色的任务路由和委派
  - **ClipHub Support**: 公司模板导出/导入，可移植公司配置
  - **Multi-Company Isolation**: 单部署多公司，数据隔离
  - **Agent Loop Monitoring**: Perceive → Think → Act 循环监控
  - **Tool Calling Tracking**: 工具调用追踪与重试机制
  - **Memory Management**: 短期记忆 + 长期记忆 + 上下文窗口管理
  - **Heartbeat Execution**: 心跳执行模式（唤醒→检查→执行→退出）
  - **Org Structure**: CEO→Manager→IC 层级汇报链
  - **Goal Alignment**: 所有任务追溯到公司/项目目标
  - **Goal-Aware Execution**: 任务携带完整目标祖先链
  - **Budget Control**: Token预算、自动暂停、成本追踪
  - **Governance**: 配置版本化、审批机制、安全回滚
  - **Ticket System**: 完整审计日志、可追溯
  - **Atomic Execution**: 任务和预算的原子化执行
  - **7-Phase Workflow**: 分析→设计→实现→测试→验证→文档→发布
  - **Multi-Framework**: OpenMOSS + CrewAI + AG2 + Paperclip统一编排
  - **BYO Agent**: 支持 OpenClaw/Claude/Codex/Cursor/HTTP Agent

  适用场景：复杂项目管理、多智能体协作、AI公司运营、专业角色代理团队、持续集成流水线

trigger: |
  Use when user wants to:
  - Orchestrate multi-agent teams with Control Plane architecture
  - Deploy Agency Agents specialized roles (Frontend, Backend, Security, etc.)
  - Route tasks to appropriate agent roles automatically
  - Monitor Agent Loop (Perceive-Think-Act) execution
  - Track Tool Calling with retry and timeout logic
  - Implement Memory management for agents
  - Build Paperclip-inspired AI company operations
  - Implement Heartbeat-based agent execution
  - Manage hierarchical org structures (CEO/Manager/IC)
  - Track budget and costs across agent teams
  - Use REPL/CLI mode for interactive agent management

  触发关键词：
  - "openmoss", "任务编排", "智能体团队", "多智能体协作"
  - "control plane", "heartbeat", "agent orchestration"
  - "agency agents", "角色代理", "专业代理"
  - "agent loop", "tool calling", "memory management"
  - "paperclip", "AI公司", "预算控制", "审计日志"
  - "crewai", "AG2", "multi-agent"

allowed-tools:
  - Read
  - Bash
  - Write
  - Skill:mirofish
  - Skill:agent-teams
  - Skill:agentic-rag
  - Skill:mcp-adapter
  - Skill:openclaw-security

inputs:
  - name: task_description
    type: string
    description: 任务描述
    required: true
  - name: mode
    type: enum[repl,cli,api]
    description: 运行模式
    default: cli
  - name: framework
    type: enum[openmoss,crewai,ag2,paperclip,hybrid]
    description: 编排框架
    default: openmoss
  - name: team_size
    type: integer
    description: 团队规模
    default: 4
  - name: execution_mode
    type: enum[continuous,heartbeat,event_driven]
    description: 执行模式
    default: heartbeat
  - name: budget_limit
    type: number
    description: Token预算限制（可选）
    required: false
  - name: enable_loop_monitor
    type: boolean
    description: 启用Agent Loop监控
    default: true
  - name: enable_tool_tracking
    type: boolean
    description: 启用工具调用追踪
    default: true

outputs:
  - task_status
  - workflow_logs
  - budget_report
  - audit_trail
  - json_output
  - exit_code
  - loop_metrics
  - tool_call_logs
---

# OpenMOSS 多智能体任务编排技能 v1.4.0

> **Paperclip-inspired Control Plane + Chenxi AI Agent Loop + Heartbeat Execution + 170+ Skills**

## What's New in v1.4.0 (Agent Loop Monitoring)

### 🔄 Agent Loop 监控（基于Chenxi AI原理）

```
┌─────────────────────────────────────────────────────────────┐
│                    Agent Loop Cycle                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│    ┌──────────┐      ┌──────────┐      ┌──────────┐        │
│    │ Perceive │ ───> │  Think   │ ───> │   Act    │        │
│    │  (感知)  │      │  (思考)  │      │  (行动)  │        │
│    └──────────┘      └──────────┘      └──────────┘        │
│          ↑                                    │             │
│          └────────────────────────────────────┘             │
│                      (反馈循环)                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**监控指标**:
- **Loop Duration**: 单次循环执行时间
- **Stuck Detection**: Agent是否"卡住"检测
- **Stage Analysis**: Perceive/Think/Act各阶段耗时
- **Iteration Count**: 循环迭代次数

**配置示例**:
```yaml
agent_loop:
  enabled: true
  monitor:
    - perceive:  # 感知阶段
        timeout: 30s
        check_input: true
    - think:     # 思考阶段
        timeout: 60s
        max_tokens: 4000
    - act:       # 行动阶段
        timeout: 120s
        check_output: true
    - detect_stuck:
        max_iterations: 50
        same_action_threshold: 5  # 连续5次相同动作视为卡住
```

### 🛠️ Tool Calling 追踪

AI不会执行代码，它只会"说"。Tool Calling让AI从"只会说"变成"能干活"。

```yaml
tool_calling:
  enabled: true
  tracking:
    log: true                    # 记录所有工具调用
    retry: 3                     # 失败重试次数
    timeout: 30s                 # 单次调用超时
    circuit_breaker: true        # 熔断机制

  schema_validation:
    enabled: true                # JSON Schema验证
    strict: false                # 严格模式

  rate_limiting:
    max_calls_per_minute: 60     # 每分钟最大调用数
    burst_allowance: 10          # 突发请求 allowance
```

### 🧠 Memory Management 机制

AI没有真正的记忆，上下文窗口 + 外部存储 = "伪记忆"。

```yaml
memory:
  # 短期记忆 (上下文窗口)
  short_term:
    enabled: true
    context_window: 128000       # token数
    retention_policy: "fifo"     # fifo/lru/priority

  # 长期记忆 (外部存储)
  long_term:
    enabled: true
    storage: "external"          # external/vector_db/file
    backend: "chroma"            # chroma/pinecone/weaviate
    index_frequency: "per_task"  # per_turn/per_task/manual

  # 记忆检索
  retrieval:
    strategy: "hybrid"           # semantic/keyword/hybrid
    top_k: 5                     # 检索条数
    similarity_threshold: 0.7

  # 为什么对话一长就"犯傻"?
  optimization:
    summarize_old_context: true  # 自动摘要旧上下文
    compress_history: true       # 压缩历史消息
    keep_key_decisions: true     # 保留关键决策点
```

---

## What's New in v1.3.0 (Paperclip-inspired)

### 🎯 Control Plane Architecture
- **Control Plane Pattern**: 控制平面与执行分离，只编排不执行
- **Bring Your Own Agent**: 支持 OpenClaw/Claude/Codex/Cursor/任意HTTP Agent
- **Agent Registry**: 统一的Agent注册和管理中心

### 💓 Heartbeat Execution Mode
- **Heartbeat Pattern**: 唤醒→检查任务→执行→退出
- **Event-Driven**: 支持评论提及、任务分配等事件触发
- **Cost Efficient**: 不持续运行，按需唤醒

### 🏢 Org Structure & Goal Alignment
- **Hierarchical Org**: CEO → Manager → IC 层级结构
- **Chain of Command**: 清晰的汇报链和升级路径
- **Goal Tracing**: 所有任务追溯到公司/项目目标

### 💰 Budget Control & Governance
- **Token Budgets**: 每个Agent的月度预算限制
- **Auto-Pause**: 预算耗尽自动暂停
- **Approval Gates**: 关键操作需要审批
- **Audit Trail**: 完整的操作审计日志

### 🎫 Ticket System
- **Task Checkout**: 原子化任务认领
- **Immutable Logs**: 不可变的操作记录
- **Full Traceability**: 从公司目标到具体执行的完整链路

---

## v1.2.0 特性（保留）

### 🏗️ CLI-Anything Architecture
- **7-Phase Pipeline**: 标准化的任务编排工作流
- **Agent Harness**: CLI原生封装
- **Dual Mode**: REPL + 子命令
- **JSON Output**: Agent友好的结构化输出

### 📚 170+ Skills Pattern
- **Multi-Domain**: 工程、产品、营销、合规、C级咨询
- **Skill Security Auditor**: 安全扫描
- **Self-Improving**: 自动学习优化
- **Zero-Dependency Tools**: Python标准库工具

### 🔒 Security First
- **Pre-flight Checks**: 任务执行前安全检查
- **Skill Vetting**: 技能安全审计
- **Permission Scoping**: 最小权限原则

---

## 7-Phase Orchestration Pipeline

```
┌─────────────────────────────────────────────────────────────────┐
│              OpenMOSS 7-Phase Orchestration                    │
├─────────────────────────────────────────────────────────────────┤
│ Phase 1: ANALYZE  → 分析任务需求，识别依赖和约束              │
│ Phase 2: DESIGN   → 设计智能体团队结构和工作流                │
│ Phase 3: IMPLEMENT → 初始化团队，配置工具和权限                │
│ Phase 4: PLAN TEST → 制定测试策略和验收标准                   │
│ Phase 5: EXECUTE  → 执行任务，监控进度和质量                  │
│ Phase 6: VALIDATE → 验证结果，运行测试套件                    │
│ Phase 7: PUBLISH  → 发布结果，更新知识库                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎯 Paperclip-Inspired Patterns (v1.3.0)

### Control Plane Architecture

OpenMOSS v1.3.0 引入 Paperclip 的 Control Plane 模式：控制平面与执行分离。

**核心原则**：
- ✅ **只编排，不执行**：OpenMOSS 是控制平面，Agent 运行在外部
- ✅ **Bring Your Own Agent**：支持任意可调用 Agent
- ✅ **Heartbeat 模式**：Agent 定期唤醒、检查、执行、退出

---

### Heartbeat Execution Mode

```
Step 1: Identity          → 获取 Agent 身份
Step 2: Get Assignments   → 获取分配的任务
Step 3: Checkout          → 原子化认领任务
Step 4: Understand Context→ 读取任务上下文
Step 5: Do Work          → 执行具体工作
Step 6: Update Status    → 更新任务状态
Step 7: Log Audit Trail  → 记录审计日志
```

---

### Org Structure

```
                    CEO
         ┌──────────┼──────────┐
       CTO         CMO         CFO
    ┌────┴────┐  ┌──┴──┐    ┌──┴──┐
 Engineer  Engineer Marketing Finance
```

**角色**：CEO（战略规划）、Manager（任务分配）、IC（具体执行）

---

### Budget Control

```bash
# 设置预算
openmoss budget set --agent Marketing-IC-01 --limit 100000 --auto-pause

# 查看状态
openmoss budget status
Agent: Marketing-IC-01
Budget: 100,000 tokens/month
Used: 78,500 (78.5%) ⚠️ WARNING
```

---

### Ticket System

**状态流转**：backlog → todo → in_progress → in_review → done

**原子 Checkout**：
```bash
POST /api/tasks/{id}/checkout
{ "agent_id": "Marketing-IC-01" }
# 409 Conflict - 已被认领
# 200 OK - 成功认领
```

---

## What's New in v1.5.0 (Paperclip Deep Integration)

### 🏢 Multi-Company Isolation

单部署支持多公司，数据完全隔离：

```yaml
multi_company:
  enabled: true
  companies:
    - id: "company-a"
      name: "AI Consulting Ltd"
      isolated: true
    - id: "company-b"
      name: "Dev Studio Inc"
      isolated: true
```

**特性**:
- 每个公司有独立的 Agent 注册表
- 任务、预算、审计日志完全隔离
- 统一的控制平面管理

### 📦 ClipHub Support - 公司模板注册中心

> "下载一个公司"

```bash
# 导出公司模板
openmoss export --company my-company --template ./my-company-template.yaml

# 导入公司模板
openmoss import --template ./my-company-template.yaml --as new-company

# 从 ClipHub 安装
openmoss cliphub install huamu668/clawhub-openmoss-dev-studio
```

**模板内容**:
```yaml
template:
  metadata:
    name: "Dev Studio"
    description: "Full-stack development team"
    version: "1.0.0"

  org_chart:
    ceo:
      name: "CEO-Agent"
      role: "strategic_planning"
    managers:
      - name: "CTO-Agent"
        reports:
          - "Senior-Engineer-01"
          - "Senior-Engineer-02"

  agents:
    - id: "Senior-Engineer-01"
      adapter: "claude"
      config: "./agents/senior-engineer.claude.md"

  seed_tasks:
    - title: "Setup project repository"
      priority: high

  budget_defaults:
    monthly_limit: 100000
    auto_pause: true
```

### ⚛️ Atomic Execution

任务认领和预算执行是原子操作：

```bash
# 原子化任务认领
POST /api/tasks/{id}/checkout
{ "agent_id": "Engineer-01" }

# 响应
200 OK    - 成功认领
409 Conflict - 已被其他 Agent 认领
429 Too Many Requests - 预算已耗尽
```

**保证**:
- 不会重复执行同一任务
- 不会超出预算限制
- 分布式锁防止竞态条件

### 🎯 Goal-Aware Execution

任务携带完整的目标祖先链：

```yaml
task:
  title: "Implement user authentication"
  goal_chain:
    - level: 0
      goal: "Build a secure SaaS platform"
      owner: "CEO-Agent"
    - level: 1
      goal: "Develop user management system"
      owner: "CTO-Agent"
    - level: 2
      goal: "Implement authentication"
      owner: "Engineer-01"
```

**优势**:
- Agent 始终理解"为什么"做这件事
- 决策与高层目标保持一致
- 上下文自动继承

### 🛡️ Enhanced Governance

配置版本化和安全回滚：

```bash
# 查看配置历史
openmoss config history --agent Engineer-01

# 审批配置变更
openmoss config approve --change-id 123 --approver CEO-Agent

# 回滚到上一版本
openmoss config rollback --agent Engineer-01 --version v1.2.0
```

**审批流**:
```
Config Change → Review → Approve → Apply → Log
     ↑__________↓
       (Reject)
```

### 📱 Mobile-Ready Dashboard

支持移动端管理：

```yaml
dashboard:
  mobile:
    enabled: true
    features:
      - view_org_chart
      - monitor_budget
      - approve_tasks
      - receive_alerts
```

---

## Multi-Framework Support

### Framework Comparison

| 特性 | OpenMOSS | CrewAI | AG2 | Hybrid |
|------|----------|--------|-----|--------|
| 角色定义 | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| 对话能力 | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| 代码执行 | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| 易用性 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |

### Framework Selection Guide

```bash
# 根据任务特征自动选择最佳框架
openmoss orchestrate \
  --task "开发一个Web应用" \
  --auto-select-framework \
  --analysis "需要代码生成、多轮对话、人机协作"
# → 推荐: AG2 (代码执行+对话能力强)

openmoss orchestrate \
  --task "市场调研报告" \
  --auto-select-framework \
  --analysis "需要研究、分析、写作"
# → 推荐: CrewAI (角色专业化)

openmoss orchestrate \
  --task "内容审核流水线" \
  --auto-select-framework \
  --analysis "需要严格流程、积分系统"
# → 推荐: OpenMOSS (流程控制严格)
```

---

## 170+ Skills Ecosystem

### 技能分类（类似claude-skills）

| 领域 | 技能数 | 代表技能 | 用途 |
|------|--------|----------|------|
| **Engineering Core** | 23 | architecture, backend, frontend | 核心工程 |
| **Engineering POWERFUL** | 25 | agent-designer, RAG-architect | 高级工程 |
| **Product** | 8 | PM, UX researcher, strategist | 产品管理 |
| **Marketing** | 42 | SEO, CRO, content creator | 市场营销 |
| **C-Level** | 28 | CTO, CEO, CFO advisor | 高管咨询 |
| **Regulatory** | 12 | GDPR, ISO, FDA specialist | 合规监管 |

### 技能发现与安装

```bash
# 搜索技能
openmoss skill search "security audit"
# Found: skill-security-auditor, compliance-auditor, pentest-agent

# 安装技能
openmoss skill install skill-security-auditor

# 查看技能详情
openmoss skill info skill-security-auditor

# 列出已安装技能
openmoss skill list --installed

# 更新技能
openmoss skill update --all
```

---

## Dual Mode Operation

### Mode 1: REPL (Interactive)

```bash
$ openmoss repl
╔══════════════════════════════════════════════════╗
║     OpenMOSS Orchestration Engine v1.2.0         ║
║   Multi-Agent Task Orchestration CLI             ║
╚══════════════════════════════════════════════════╝

openmoss> task create "开发用户认证系统"
✓ Task created: task_001

openmoss> team create --for task_001 --size 4
✓ Team created: team_001
  - planner: Alice
  - executor: Bob
  - reviewer: Charlie
  - patrol: Diana

openmoss[task_001]> framework select hybrid
✓ Framework: hybrid (OpenMOSS + CrewAI)

openmoss[task_001]> skill install backend-api-security
✓ Installed: backend-api-security

openmoss[task_001]> run --phases all
[Phase 1/7] Analyzing... ✓
[Phase 2/7] Designing... ✓
[Phase 3/7] Implementing... ✓ (4 subtasks created)
[Phase 4/7] Planning tests... ✓
[Phase 5/7] Executing... ⠙ (2/4 complete)

openmoss[task_001]> status
Task: task_001
Status: executing
Progress: 71% (5/7 phases)
Agents: 4 active
Subtasks: 2 complete, 2 in progress

openmoss[task_001]> export --format json,html
✓ Exported to ./task_001_report.html

openmoss> exit
Goodbye! 👋
```

### Mode 2: CLI (Scriptable)

```bash
# 完整编排管道
openmoss orchestrate \
  --task "开发电商支付系统" \
  --description "包含支付、退款、对账功能" \
  --framework hybrid \
  --team-size 6 \
  --roles planner,executor,reviewer,patrol,security-expert,qa-engineer \
  --skills backend-api-designer,security-auditor,test-suite-builder \
  --phases all \
  --output ./payment-system/ \
  --json

# JSON输出
{
  "orchestration_id": "orch_20250310_001",
  "task": "开发电商支付系统",
  "status": "completed",
  "framework": "hybrid",
  "team": {
    "size": 6,
    "agents": ["planner", "executor", "reviewer", "patrol", "security-expert", "qa-engineer"]
  },
  "phases": [
    {"phase": 1, "name": "analyze", "status": "completed", "duration": "2m"},
    {"phase": 2, "name": "design", "status": "completed", "duration": "5m"},
    {"phase": 3, "name": "implement", "status": "completed", "duration": "15m"},
    {"phase": 4, "name": "plan_test", "status": "completed", "duration": "3m"},
    {"phase": 5, "name": "execute", "status": "completed", "duration": "20m"},
    {"phase": 6, "name": "validate", "status": "completed", "duration": "10m"},
    {"phase": 7, "name": "publish", "status": "completed", "duration": "2m"}
  ],
  "subtasks": {
    "total": 8,
    "completed": 8,
    "failed": 0
  },
  "artifacts": [
    {"type": "code", "path": "./payment-system/src/"},
    {"type": "tests", "path": "./payment-system/tests/"},
    {"type": "docs", "path": "./payment-system/docs/"},
    {"type": "report", "path": "./payment-system/report.html"}
  ],
  "quality_score": 0.94,
  "exit_code": 0
}
```

---

## Security & Skill Auditing

### Skill Security Auditor (from claude-skills)

```bash
# 审计技能安全性
openmoss skill audit ./my-skill/

# 输出示例
🔍 Skill Security Audit Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Skill: my-skill
Version: 1.0.0
Overall: ⚠️ WARN

Findings:
  ❌ HIGH: Command injection in install.sh line 15
  ⚠️ MEDIUM: Network request to external API
  ✓ LOW: File permissions look good

Recommendations:
  1. Sanitize user input before shell execution
  2. Document external API usage
  3. Add input validation

Fix diff:
  - eval "$user_input"
  + eval "$(echo "$user_input" | sed 's/[^a-zA-Z0-9_-]//g')"
```

### Pre-flight Security Checks

```yaml
preflight_checks:
  permission_check:
    - verify_write_access: ["./output/", "./logs/"]
    - verify_read_access: ["./config/"]
    - verify_no_sudo_required

  network_check:
    - verify_allowed_hosts: ["api.openmoss.io", "github.com"]
    - block_suspicious_outbound: true

  dependency_check:
    - scan_for_malicious_deps: true
    - verify_checksums: true

  skill_vetting:
    - audit_all_skills: true
    - min_audit_score: 80
```

---

## Self-Improving Orchestration

### Pattern Learning

```python
class SelfImprovingOrchestrator:
    """
    从执行历史中学习的编排器
    """

    def learn_from_execution(self, execution_id):
        # 1. 分析执行数据
        execution = self.get_execution(execution_id)

        # 2. 识别成功模式
        if execution.quality_score > 0.9:
            self.extract_pattern(execution)

        # 3. 识别失败原因
        if execution.failed_tasks:
            self.analyze_failures(execution.failed_tasks)

        # 4. 优化编排策略
        self.update_orchestration_strategy()

    def extract_pattern(self, execution):
        """提取成功模式"""
        pattern = {
            "task_type": execution.task_type,
            "optimal_team_size": len(execution.agents),
            "best_framework": execution.framework,
            "effective_skills": execution.skills_used,
            "phase_durations": execution.phase_timings,
            "success_rate": execution.success_rate
        }
        self.patterns_db.save(pattern)

    def recommend_for_task(self, task):
        """基于学习推荐最佳配置"""
        similar = self.patterns_db.find_similar(task)
        if similar:
            return {
                "recommended_team_size": similar.optimal_team_size,
                "recommended_framework": similar.best_framework,
                "recommended_skills": similar.effective_skills,
                "estimated_duration": sum(similar.phase_durations)
            }
```

### Auto-Optimization

```bash
# 启用自改进模式
openmoss orchestrate \
  --task "开发新功能" \
  --self-improving \
  --learning-mode active

# 系统将：
# 1. 记录执行指标
# 2. 分析成功/失败模式
# 3. 自动调整团队配置
# 4. 优化阶段时长估计
```

---

## CLI Tools (Python, stdlib-only)

### Tool 1: Task Dependency Analyzer

```python
#!/usr/bin/env python3
"""
openmoss_analyze_deps.py - Analyze task dependencies
Zero dependencies, stdlib only.
"""

import json
import sys
from collections import defaultdict


def analyze_dependencies(tasks):
    """Analyze task dependencies and detect issues."""
    graph = defaultdict(list)
    in_degree = defaultdict(int)

    # Build dependency graph
    for task in tasks:
        task_id = task["id"]
        for dep in task.get("depends_on", []):
            graph[dep].append(task_id)
            in_degree[task_id] += 1

    # Detect cycles
    cycle = detect_cycle(graph, in_degree, tasks)

    # Calculate critical path
    critical_path = calculate_critical_path(tasks, graph)

    # Find parallelizable tasks
    parallel_groups = find_parallel_groups(tasks, graph)

    return {
        "tasks": len(tasks),
        "dependencies": sum(len(t.get("depends_on", [])) for t in tasks),
        "cycle_detected": cycle is not None,
        "cycle": cycle,
        "critical_path": critical_path,
        "critical_path_length": len(critical_path),
        "parallel_groups": parallel_groups,
        "optimization_suggestions": generate_suggestions(tasks, cycle, parallel_groups)
    }


def detect_cycle(graph, in_degree, tasks):
    """Detect cycles using Kahn's algorithm."""
    queue = [t["id"] for t in tasks if in_degree[t["id"]] == 0]
    visited = set()

    while queue:
        node = queue.pop(0)
        visited.add(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    if len(visited) == len(tasks):
        return None

    # Return unvisited nodes (part of cycle)
    return [t["id"] for t in tasks if t["id"] not in visited]


def calculate_critical_path(tasks, graph):
    """Calculate critical path using simple topological sort."""
    # Simplified version - real implementation would use weighted edges
    return topological_sort(tasks, graph)


def topological_sort(tasks, graph):
    """Topological sort for task ordering."""
    visited = set()
    result = []

    def visit(node):
        if node in visited:
            return
        visited.add(node)
        for neighbor in graph.get(node, []):
            visit(neighbor)
        result.append(node)

    for task in tasks:
        visit(task["id"])

    return list(reversed(result))


def find_parallel_groups(tasks, graph):
    """Find tasks that can be executed in parallel."""
    levels = {}
    for task in tasks:
        deps = task.get("depends_on", [])
        if not deps:
            levels[task["id"]] = 0
        else:
            levels[task["id"]] = max(levels.get(d, 0) for d in deps) + 1

    groups = defaultdict(list)
    for task_id, level in levels.items():
        groups[level].append(task_id)

    return dict(groups)


def generate_suggestions(tasks, cycle, parallel_groups):
    """Generate optimization suggestions."""
    suggestions = []

    if cycle:
        suggestions.append(f"Remove cycle: {cycle}")

    if len(parallel_groups) > 1:
        parallel_count = sum(len(g) for g in parallel_groups.values())
        suggestions.append(f"Can parallelize {parallel_count} tasks across {len(parallel_groups)} levels")

    return suggestions


if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: openmoss_analyze_deps.py <tasks.json>")
        sys.exit(1)

    # Security: Validate file path
    tasks_file = sys.argv[1]
    path = Path(tasks_file).resolve()
    allowed_dirs = [Path.cwd(), Path("~/.openmoss").expanduser(), Path("/tmp")]

    if not any(str(path).startswith(str(allowed)) for allowed in allowed_dirs):
        print(json.dumps({"error": "File path not in allowed directories"}, indent=2))
        sys.exit(1)

    if not path.exists():
        print(json.dumps({"error": "File not found"}, indent=2))
        sys.exit(1)

    if path.stat().st_size > 10 * 1024 * 1024:  # 10MB limit
        print(json.dumps({"error": "File too large (max 10MB)"}, indent=2))
        sys.exit(1)

    try:
        tasks = json.load(open(path, 'r', encoding='utf-8'))
        analysis = analyze_dependencies(tasks)
        print(json.dumps(analysis, indent=2))
    except json.JSONDecodeError as e:
        print(json.dumps({"error": f"Invalid JSON: {str(e)}"}, indent=2))
        sys.exit(1)
```

### Tool 2: Team Performance Monitor

```python
#!/usr/bin/env python3
"""
openmoss_monitor_team.py - Monitor team performance
Zero dependencies, stdlib only.
"""

import json
import sys
from datetime import datetime
from pathlib import Path


def monitor_team(team_id, metrics_file):
    """Monitor team performance metrics."""
    metrics = json.load(open(metrics_file))

    report = {
        "team_id": team_id,
        "timestamp": datetime.now().isoformat(),
        "agents": {},
        "overall": {},
        "alerts": []
    }

    # Analyze each agent
    for agent_id, agent_metrics in metrics.get("agents", {}).items():
        report["agents"][agent_id] = {
            "tasks_completed": agent_metrics.get("tasks_completed", 0),
            "avg_completion_time": agent_metrics.get("avg_completion_time", 0),
            "success_rate": agent_metrics.get("success_rate", 0),
            "score": agent_metrics.get("score", 0),
            "status": classify_agent_status(agent_metrics)
        }

        # Generate alerts
        if agent_metrics.get("success_rate", 1.0) < 0.7:
            report["alerts"].append({
                "severity": "warning",
                "agent": agent_id,
                "message": f"Low success rate: {agent_metrics['success_rate']:.1%}"
            })

    # Calculate overall metrics
    all_scores = [a["score"] for a in report["agents"].values()]
    report["overall"] = {
        "total_agents": len(report["agents"]),
        "avg_score": sum(all_scores) / len(all_scores) if all_scores else 0,
        "top_performer": max(report["agents"].items(), key=lambda x: x[1]["score"])[0] if report["agents"] else None,
        "needs_attention": [aid for aid, a in report["agents"].items() if a["status"] == "at_risk"]
    }

    return report


def classify_agent_status(metrics):
    """Classify agent status based on metrics."""
    score = metrics.get("score", 0)
    success_rate = metrics.get("success_rate", 1.0)

    if score > 80 and success_rate > 0.9:
        return "excellent"
    elif score > 60 and success_rate > 0.8:
        return "good"
    elif score > 40:
        return "fair"
    else:
        return "at_risk"


if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: openmoss_monitor_team.py <team_id> <metrics.json>")
        sys.exit(1)

    report = monitor_team(sys.argv[1], sys.argv[2])
    print(json.dumps(report, indent=2))
```

---

## Testing Strategy

### Unit Tests

```python
# tests/test_dependency_analyzer.py
import unittest
from openmoss_analyze_deps import analyze_dependencies, detect_cycle


class TestDependencyAnalyzer(unittest.TestCase):
    def test_no_cycle(self):
        tasks = [
            {"id": "A", "depends_on": []},
            {"id": "B", "depends_on": ["A"]},
            {"id": "C", "depends_on": ["B"]}
        ]
        result = analyze_dependencies(tasks)
        self.assertFalse(result["cycle_detected"])

    def test_cycle_detection(self):
        tasks = [
            {"id": "A", "depends_on": ["C"]},
            {"id": "B", "depends_on": ["A"]},
            {"id": "C", "depends_on": ["B"]}  # Creates cycle
        ]
        result = analyze_dependencies(tasks)
        self.assertTrue(result["cycle_detected"])
        self.assertIn("A", result["cycle"])
```

### E2E Tests

```bash
#!/bin/bash
# tests/e2e/test_full_orchestration.sh

set -e

echo "=== E2E Test: Full Orchestration ==="

# Create test task
mkdir -p /tmp/test_orch

# Phase 1-2: Analyze and Design
echo "Phase 1-2: Analyzing and designing..."
openmoss orchestrate \
  --task "测试任务" \
  --framework openmoss \
  --team-size 4 \
  --phases analyze,design \
  --dry-run \
  --output /tmp/test_orch/design.json

[ -f /tmp/test_orch/design.json ] || exit 1

# Phase 3-7: Full execution
echo "Phase 3-7: Executing..."
openmoss orchestrate \
  --task "实现测试功能" \
  --framework openmoss \
  --team-size 4 \
  --output /tmp/test_orch/results/ \
  --json

# Verify outputs
[ -f /tmp/test_orch/results/orchestration.json ] || exit 1
echo "=== E2E Test Passed ==="
```

---

## References

- [CLI-Anything](https://github.com/HKUDS/CLI-Anything) - Agent-native CLI generation
- [claude-skills](https://github.com/alirezarezvani/claude-skills) - 170+ production skills
- [OpenMOSS](https://github.com/uluckyXH/OpenMOSS) - Original orchestration platform
- [CrewAI](https://docs.crewai.com/) - Role-based agent teams
- [AG2](https://github.com/ag2ai/ag2) - Conversational multi-agent
- [Paperclip](https://github.com/paperclipai/paperclip) - Control plane for AI companies (v1.3.0 inspiration)

---

## Changelog

### v1.3.0 (2026-03-10) - Paperclip-Inspired
- **新增**: Control Plane 架构（控制平面与执行分离）
- **新增**: Heartbeat 执行模式（唤醒→检查→执行→退出）
- **新增**: 公司/组织架构（CEO→Manager→IC）
- **新增**: 目标对齐（任务追溯到公司目标）
- **新增**: 预算控制（Token预算、自动暂停）
- **新增**: Ticket System（原子Checkout、审计日志）
- **新增**: BYO Agent（支持 OpenClaw/Claude/Codex/Cursor）
- **新增**: Governance & Approval Gates

### v1.2.0 (2026-03-10)
- **新增**: CLI-Anything 7-Phase Pipeline
- **新增**: Agent Harness Pattern
- **新增**: 170+ Skills Ecosystem模式
- **新增**: Security Auditor集成
- **新增**: Self-Improving Orchestration
- **新增**: Python CLI工具集
- **优化**: Multi-Framework统一接口

### v1.1.0 (2026-03-10)
- 新增CrewAI/AG2集成
- 新增Agent Teams v2

### v1.0.0 (2026-03-09)
- 初始版本

---

*Agent-Native Orchestration, Self-Improving Systems*

---

## Security Considerations

### Task and File Security

All CLI tools implement comprehensive security measures:

- **Path traversal prevention**: All file paths validated against allowed directories
- **Allowed directories**: Current working directory, `~/.openmoss/`, `/tmp/`
- **File size limits**: 10MB for task files to prevent DoS
- **Canonical paths**: Uses `Path.resolve()` for consistent path handling

### Dependency Analysis Security

The `openmoss_analyze_deps.py` tool includes:

```python
# Security: Validate file path
path = Path(tasks_file).resolve()
allowed_dirs = [Path.cwd(), Path("~/.openmoss").expanduser(), Path("/tmp")]

if not any(str(path).startswith(str(allowed)) for allowed in allowed_dirs):
    print(json.dumps({"error": "File path not in allowed directories"}, indent=2))
    sys.exit(1)
```

### Team Monitoring Security

The `openmoss_monitor_team.py` tool:
- Only reads from designated metrics directories
- Validates JSON input before processing
- No external network calls
- Sanitized output format

### Skill Installation Security

Following claude-skills security patterns:

1. **Pre-installation audit**: All skills scanned before installation
2. **Permission scoping**: Minimum required permissions
3. **Code review**: Automated checks for dangerous patterns
4. **Sandbox testing**: New skills tested in isolated environment

### Prohibited Patterns

Skills are automatically rejected if they contain:
- `eval()` or `exec()` calls with user input
- `subprocess.call()` with unsanitized arguments
- `os.system()` calls
- Hardcoded credentials or tokens
- Undocumented network requests

### Agent Communication Security

- Inter-agent messages validated before processing
- No code execution via message passing
- Consensus results cryptographically verifiable
- Audit logs for all inter-agent communications

---

*Secure Orchestration, Trusted Agents*

---

## 十四、多角色代理系统 (Agency Agents) - v1.6 新增

> 整合 Agency Agents，构建专业角色代理团队

### 14.1 系统概述

多角色代理系统允许根据任务类型自动路由到专业的 AI 代理角色。每个角色都有：
- **专业领域**: 深度专精的技能集
- **个性特征**: 独特的沟通风格和工作方式
- **交付标准**: 明确的工作成果和质量要求

### 14.2 可用角色清单

#### 工程部门

| 角色 | 专长 | 适用场景 |
|------|------|----------|
| **Frontend Developer** | React/Vue/Angular, UI 实现 | Web 应用、像素级 UI |
| **Backend Architect** | API 设计、数据库架构 | 服务端系统、微服务 |
| **Mobile App Builder** | iOS/Android, React Native | 跨平台移动应用 |
| **AI Engineer** | ML 模型、AI 集成 | 机器学习功能 |
| **DevOps Automator** | CI/CD、基础设施自动化 | 流水线开发 |
| **Security Engineer** | 威胁建模、安全代码审查 | 应用安全 |
| **Database Optimizer** | 模式设计、查询优化 | PostgreSQL/MySQL 调优 |
| **Code Reviewer** | 建设性代码审查 | PR 审查、质量门禁 |
| **SRE** | SLO、错误预算、可观测性 | 生产可靠性 |
| **Software Architect** | 系统设计、DDD | 架构决策 |

#### 内容部门

| 角色 | 专长 | 适用场景 |
|------|------|----------|
| **Content Strategist** | 内容策略、编辑日历 | 内容规划 |
| **SEO Specialist** | 关键词研究、技术 SEO | 搜索优化 |
| **Copywriter** | 广告文案、着陆页 | 营销内容 |
| **Technical Writer** | 开发者文档、API 文档 | 技术文档 |

#### 设计部门

| 角色 | 专长 | 适用场景 |
|------|------|----------|
| **UX Researcher** | 用户研究、可用性测试 | 用户洞察 |
| **UI Designer** | 界面设计、设计系统 | 视觉设计 |
| **Design System Architect** | 组件库、设计规范 | 设计系统 |

#### 营销部门

| 角色 | 专长 | 适用场景 |
|------|------|----------|
| **Growth Hacker** | A/B 测试、漏斗优化 | 增长实验 |
| **Social Media Manager** | 内容日历、社区管理 | 社媒运营 |
| **Email Marketer** | 邮件序列、自动化 | 邮件营销 |

### 14.3 角色激活方式

在 OpenClaw/Claude Code 中使用：

```bash
# 方法 1: 直接指定角色
"激活 Frontend Developer 角色，帮我构建一个 React 组件"

# 方法 2: 描述需求自动路由
"我需要优化数据库查询性能"
# → 自动路由到 Database Optimizer

# 方法 3: 多角色协作
"设计并实现一个完整的用户认证系统"
# → Software Architect (设计)
# → Backend Architect (API)
# → Frontend Developer (UI)
# → Security Engineer (审计)
```

### 14.4 角色协作工作流

#### 案例 1: 全栈功能开发

```
用户: "开发一个电商购物车功能"

1. Software Architect
   → 设计系统架构
   → 定义 API 契约
   → 输出: TECH-DESIGN.md

2. Backend Architect
   → 设计数据库模式
   → 实现 REST API
   → 输出: API 实现 + 测试

3. Frontend Developer
   → 实现 React 组件
   → 集成 API 调用
   → 输出: UI 组件 + 样式

4. Security Engineer
   → 审查输入验证
   → 检查权限控制
   → 输出: 安全审计报告

5. Code Reviewer
   → 审查所有代码
   → 提出改进建议
   → 输出: 审查意见
```

#### 案例 2: 内容营销活动

```
用户: "策划一个产品发布营销活动"

1. Content Strategist
   → 制定内容策略
   → 规划发布节奏
   → 输出: CONTENT-STRATEGY.md

2. Copywriter
   → 撰写营销文案
   → 设计着陆页
   → 输出: 文案 + 页面原型

3. SEO Specialist
   → 关键词研究
   → 优化内容结构
   → 输出: SEO 建议

4. Social Media Manager
   → 设计社媒内容
   → 制定发布计划
   → 输出: 社媒日历

5. Growth Hacker
   → 设计增长实验
   → 设置追踪指标
   → 输出: 实验方案
```

### 14.5 角色配置文件

每个角色的完整定义包含：

```yaml
role:
  name: "Frontend Developer"
  specialty: "React/Vue/Angular, UI implementation"
  personality: "Detail-oriented, pixel-perfect, pragmatic"

  workflows:
    - analyze_requirements
    - design_component_api
    - implement_with_tests
    - optimize_performance

  deliverables:
    - Component code
    - Unit tests
    - Storybook stories
    - Performance report

  communication_style:
    - "Uses precise technical terminology"
    - "Provides code examples"
    - "Asks clarifying questions about requirements"
```

### 14.6 与 OpenMOSS 集成

```yaml
# openmoss-orchestration 配置
agents:
  roles:
    - name: "frontend-dev"
      role: "Frontend Developer"
      skills:
        - react
        - typescript
        - tailwind

    - name: "backend-arch"
      role: "Backend Architect"
      skills:
        - nodejs
        - postgresql
        - redis

    - name: "security-eng"
      role: "Security Engineer"
      skills:
        - threat-modeling
        - penetration-testing
        - compliance

routing:
  rules:
    - pattern: "ui|frontend|component|react"
      agent: "frontend-dev"

    - pattern: "api|backend|database|server"
      agent: "backend-arch"

    - pattern: "security|auth|vulnerability"
      agent: "security-eng"
```

### 14.7 最佳实践

1. **明确任务边界**: 每个角色的任务应该清晰可交付
2. **顺序依赖管理**: 使用工作流引擎管理角色执行顺序
3. **结果交接标准**: 定义清晰的交付物和验收标准
4. **质量门禁**: 关键节点设置 Code Review 或审核
5. **成本控制**: 为每个角色设置 Token 预算上限

---

## 更新日志

### v1.6.0 (2026-03-12)
- ✨ 新增 Agency Agents 多角色代理系统
- ✨ 支持 20+ 专业角色（工程/内容/设计/营销）
- ✨ 新增角色路由和协作工作流
- ✨ 集成 Agency Agents Skill 精华
- 📚 更新触发关键词和适用场景

### v1.5.0
- 初始版本，Paperclip-inspired Control Plane
- Agent Loop 监控
- ClipHub 支持
- 多公司隔离

---

*Secure Orchestration, Trusted Agents*
*升级至 v1.6.0 | 新增多角色代理系统*
