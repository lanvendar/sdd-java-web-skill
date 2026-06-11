# 结构驱动开发：让数据成为系统协作的契约

中文 | [English](./README.en.md)

`sdd-java-web` 是一个面向 Java Web 项目的结构驱动开发 Skill。它把数据库表结构、Java 后端模型、前端页面模型和 API 契约统一拉回到同一套数据定义中，让功能开发先对齐“数据长什么样”，再推进行为设计和代码实现。

这个项目不是一个传统的 Java Web 应用，而是一套可复用的开发方法和文档模板。它适合用在 Spring Web、Controller、Service、DTO、Mapper/DAO、Entity/PO、前端页面、API Client、持久化和集成类需求中，尤其适合多人协作、字段频繁变化、前后端契约容易漂移的业务系统。

很多系统与系统之间的摩擦，本质上并不是因为代码不够聪明，而是因为它们对同一件事的结构理解不一致：数据库有一套字段，后端有一套对象，接口有一套命名，前端又有一套状态。每一层都觉得自己是合理的，但层与层之间的转换、补丁、兼容和返工会持续消耗工程效率。

如果设计者从一开始就用数据结构来定义业务对象、接口边界和流转关系，那么系统的边界会变得更清晰。数据结构不是实现细节，而是协作语言；不是某一层的附属品，而是贯穿数据库、服务、接口和界面的共同契约。

## 核心理念

**数据结构是第一张蓝图。**

在开发任何功能之前，先用 `{feature}-data-define.md` 定义数据库、后端、前端的数据模型；再用 `{feature}-design.md` 描述数据如何流动、如何校验、如何持久化、如何展示、如何被接口和服务消费。

这意味着：

- 字段、DTO、SQL、API 请求响应结构必须先出现在数据定义文档里。
- 行为、调用链、文件变更、服务逻辑、集成方式必须落在设计文档里。
- 如果实现需要偏离文档，先更新文档，再改代码。
- 不强行套 CRUD 模板，只记录真实发生的接口、服务方法和页面交互。

## 为什么从数据结构开始

传统开发常常从页面、接口或任务列表开始推进，数据结构则在实现过程中不断被补充。短期看这很快，长期看却容易产生三类摩擦：

- **跨层摩擦**：数据库字段、后端 DTO、接口返回、前端状态各自演化，联调时才发现含义、类型、枚举和必填规则不一致。
- **跨系统摩擦**：外部系统、内部模块、消息队列、文件导入导出对同一业务对象有不同结构，转换逻辑变成隐形复杂度。
- **跨团队摩擦**：产品、前端、后端、测试讨论的是同一个功能，却没有一份共同承认的结构语言，导致需求边界反复漂移。

结构驱动开发的目标，是把这些摩擦前置到设计阶段解决。先定义对象、字段、约束、来源、去向和转换规则，再讨论接口、页面、服务和存储如何围绕这些结构工作。

当数据结构稳定下来，层与层之间的边界也会自然清晰：

- 数据库负责事实存储和约束。
- 后端负责业务规则、上下文和事务。
- API 负责明确输入输出契约。
- 前端负责展示、交互和局部状态。
- 集成层负责结构转换与外部语义对齐。

每一层都可以有自己的职责，但不再各自发明“同一个业务对象”。

## 项目结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── convention-discovery.md
    ├── data-define.md
    └── design.md
```

### `SKILL.md`

Skill 的入口文件，定义了 `sdd-java-web` 的触发场景、核心规则、工作流、文档规则、实现规则和输出要求。

### `references/data-define.md`

数据定义模板，是整个方法的核心。它覆盖：

- Database Data Model
- Java Backend Data Model
- Frontend Data Model
- Data Mapping Rules
- Reused Structures

它要求明确主键、候选键、分区策略、字段注释、DTO 字段、前端查询模型、页面展示模型、表单模型、状态模型和跨层转换规则。

### `references/design.md`

功能设计模板，负责描述数据结构之外的行为部分，包括：

- 能力边界
- 数据流
- API 契约
- 文件变更
- Controller / Service / Mapper 设计
- 前端页面结构和交互
- 集成、安全、验证方式

### `references/convention-discovery.md`

项目约定发现指南。它要求在真正设计或实现功能前，先读取目标项目的 `docs/.sdd/project-index.yml` 和 `docs/.sdd/project-conventions.md`，再结合邻近代码推断模块、包结构、构建命令、API 包装、分页、错误处理、路由和持久化风格。

### `agents/openai.yaml`

Agent 展示配置，提供 Skill 的显示名称、简短描述和默认提示词。

## 工作流

1. **发现项目约定**
   读取目标项目中的 SDD 索引、项目约定、邻近代码、构建文件和已有文档。

2. **定义数据结构**
   为目标功能创建或更新 `{feature}-data-define.md`，先锁定数据库、后端、前端的数据模型。

3. **设计行为流转**
   创建或更新 `{feature}-design.md`，描述页面、API、Controller、Service、Mapper、数据库和外部系统之间的数据流。

4. **按项目风格实现**
   复用目标项目已有的包结构、响应模型、校验方式、分页约定、持久化模式和前端组件风格。

5. **最小可靠验证**
   按项目约定运行编译、测试、构建、lint 或人工验证命令，并记录结果。

## 推荐的目标项目文档布局

单模块项目：

```text
docs/
├── .sdd/
│   ├── project-index.yml
│   └── project-conventions.md
├── order-data-define.md
└── order-design.md
```

多模块项目：

```text
docs/
├── .sdd/
│   ├── project-index.yml
│   └── project-conventions.md
├── backend/
│   ├── order-data-define.md
│   └── order-design.md
└── frontend/
    └── order-page-design.md
```

复杂功能可以拆分多个设计文档，但始终保留一个数据定义文档作为唯一事实源。

## 适用场景

- 新增 Java Web 业务功能
- 改造数据库字段、DTO、VO、Form、Query 或前端展示模型
- 对齐前后端 API 契约
- 梳理 Controller、Service、Mapper/DAO、Entity/PO 调用链
- 设计跨模块、外部系统、消息、文件、缓存或定时任务集成
- 在实现前先输出清晰的设计文档
- 代码评审时检查字段、接口和实现是否偏离设计

## 设计原则

- **Single Source of Truth**：数据定义文档是结构事实源。
- **Document First, Code Second**：先更新结构和设计，再实现。
- **Local Convention First**：优先遵循目标项目已有约定。
- **Thin Controller, Rich Service**：Controller 保持薄，业务规则进入 Service。
- **No Phantom Fields**：没有进入文档的字段，不应该出现在实现里。
- **No Forced CRUD**：只定义真实需要的 API、Service 和页面行为。
- **Reuse Explicitly**：复用对象要写清楚路径、方式和边界，避免重复造模型。

## 使用方式

在目标 Java Web 项目中，让 Codex 使用 `sdd-java-web` Skill，并描述你要设计或实现的功能，例如：

```text
使用 sdd-java-web，为订单退款功能生成数据定义和设计文档。
```

或：

```text
使用 sdd-java-web，基于现有 Spring Controller、Service、Mapper 风格实现客户标签管理页面和接口。
```

Skill 会优先读取项目约定，再生成或更新功能级数据定义与设计文档；如果用户请求实现，它会继续按文档约束完成代码变更和验证。

## 这个项目解决什么问题

很多 Java Web 项目真正失控的地方，不是代码写不出来，而是字段在数据库、DTO、接口、前端状态和页面展示之间各自生长。`sdd-java-web` 的目标是把这些漂移收束起来：先让结构稳定，再让行为可靠，最后让实现自然落地。

当团队围绕同一份数据定义协作时，需求讨论会更具体，接口联调会更少猜测，代码评审也能从“你怎么写的”转向“你有没有遵守结构和流转约定”。
