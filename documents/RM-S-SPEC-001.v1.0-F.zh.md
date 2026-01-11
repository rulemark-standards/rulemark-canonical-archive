
# RuleMark 系统规范（System Specification）

## 命名 · 目录 · 模板 · 控制平面

**文档编号：** RM-S-SPEC-001
**版本：** v1.0-F
**状态：** 冻结 · 生效 · 强制执行
**适用范围：** V1（人类）· V2（机器）· V3（资本）· V4（AI）
**权威级别：** 最高 —— 所有子系统**必须**遵守

> 本文件构成 RuleMark 的**度量公约（Measurement Convention）**。
> 任何修改，等同于**宪法修正**。

---

## 第一部分：版本分层（规范性定义）

| 层级     | 受众       | 功能      | 执行方式      |
| ------ | -------- | ------- | --------- |
| **V1** | 人类 / 审计  | 法律文本、定义 | Git 不可变   |
| **V2** | 系统 / 索引器 | 元数据、解析  | 严格 Schema |
| **V3** | 合约 / 预言机 | 金融参数    | 预言机就绪     |
| **V4** | AI 智能体   | 确定性执行   | 机器摘要      |

**原则：**

* 一个**规范 ID（Canonical ID）**统御所有层级
* 版本可以演进，身份永不改变

---

## 第二部分：命名规范（标识符）

### 1. 规范 ID（逻辑实体 · 永久）

**格式**

```
[域]-[类型]-[主题]-[序号]
```

**示例**

```
RM-P-PEG-001
```

**规则**

* 用于合约、引用、对外标注
* **不得**包含版本或状态
* 一经发布，**永不改变**

---

### 2. 物理文件名（存储实体）

**格式**

```
[Canonical_ID].v[版本]-[状态].[扩展名]
```

**示例**

```
RM-P-PEG-001.v1.0-F.md
```

---

### 3. 字段定义

| 字段 | 含义   | 允许值                  |
| -- | ---- | -------------------- |
| 域  | 系统领域 | RM / GOV / OPS / INF |
| 类型 | 文档类型 | P（判例）/ S（标准）/ G（治理）  |
| 主题 | 主题码  | 3–6 位大写字母            |
| 序号 | 顺序号  | 3 位数字                |
| 状态 | 法律状态 | D / A / F            |

---

## 第三部分：目录拓扑（物理结构）

```
/rulemark-canonical-archive
│
├── /documents
│   ├── SYSTEM-SPEC-V1-V4.v1.0-F.md
│   └── ARCHITECTURE-V2.md
│
├── /precedents
│   ├── RM-P-PEG-001.v1.0-F.md
│   └── RM-P-PEG-001.v1.0-F.sig
│
├── /standards
│   ├── RM-S-RWA-001.v1.0-F.md
│   └── RM-S-RWA-001.v1.0-F.sig
│
├── /governance
│   └── CONSTITUTION-V2.md
│
├── /schemas
│   └── yaml-schema-v2.json
│
└── registry.json   ← 自动生成、不可变索引
```

---

## 第四部分：权威文档结构（模具）

### 黄金 YAML 头（强制）

**所有判例 / 标准文件必须包含**

```yaml
---
# V1 & V2：身份
id: RM-S-TOPIC-000.v1.0-F
canonical_id: RM-S-TOPIC-000
title: "标题"
type: Standard
status: Frozen
author: "RuleMark Architecture"
created: 2026-01-10

# V2：完整性与 Schema
file_integrity:
  hash: sha256:<hex>
  schema_version: "2.0"

dependencies:
  - RM-S-XXXX-000.v1.0-F

# V3：资本 / 预言机接口（仅数值/布尔/枚举/null）
oracle_data:
  min_value: 0
  is_active: true
  currency: "USDT"
  risk_level: 1

# V4：AI 确定性逻辑
machine_summary: >
  IF condition_met IS true
  THEN execute_action allowed.
  FAIL_CONDITION: collateral < min_value.
  FALLBACK → HUMAN_REVIEW.
  CONFIDENCE ≥ 0.95.
---
```

**硬性规则**

* 缺少 YAML → CI 失败
* `oracle_data` 出现自然语言 → 失败
* 缺少 `machine_summary` → 失败

---

## 第五部分：签名文件规范（.sig）

**文件名**

```
[物理文件名].sig
```

**内容（JSON，强制）**

```json
{
  "target_file": "RM-S-TOPIC-000.v1.0-F.md",
  "target_hash": "sha256:<hex>",
  "signer_id": "did:rulemark:founder",
  "signature_algorithm": "ed25519",
  "signature_value": "<hex_or_base64>",
  "timestamp": "2026-01-10T14:00:00Z"
}
```

**规则**

* 不符合该结构的签名，**不具权威性**

---

## 第六部分：权威与自动化（控制平面）

### 6.1 冻结权限

| 状态    | 所需权限                 |
| ----- | -------------------- |
| **F** | 创始人 DID（一期）或 DAO 5/7 |
| **A** | 架构委员会 3/5            |
| **D** | 开放贡献                 |

---

### 6.2 Registry 自动生成（强制）

**触发**

* Git 预提交钩子 或 CI 合并

**失败条件**

* 规范 ID 重复 → CI 失败
* 缺 YAML 或 `.sig` → CI 失败

---

### 6.3 `registry.json` 规范 Schema（冻结）

```json
{
  "metadata": {
    "generated_at": "2026-01-10T14:30:00Z",
    "total_documents": 42,
    "integrity_hash": "sha256:<按 canonical_id 排序后的哈希>"
  },
  "documents": [
    {
      "canonical_id": "RM-P-PEG-001",
      "latest_version": "v1.0-F",
      "status": "Frozen",
      "type": "Precedent",
      "file_path": "/precedents/RM-P-PEG-001.v1.0-F.md",
      "signature_path": "/precedents/RM-P-PEG-001.v1.0-F.sig",
      "last_updated": "2026-01-10T14:00:00Z"
    }
  ]
}
```

**硬性规则**

* 仅允许自动生成
* `integrity_hash` 必须匹配
* 任一不符 → 系统无效

---

### 6.4 不可变法则

1. **禁止重命名** —— 冻结文件名即法律
2. **禁止覆盖** —— 变更必须新版本
3. **快速失败** —— 不合规即停机

---

## 最终声明

本规范已**冻结**。
定义事实如何被**创建、验证、冻结与消费**，
适用于人类、机器、资本与 AI。

> 自此之后：
> **任何修改，等同宪法修正。**

---

