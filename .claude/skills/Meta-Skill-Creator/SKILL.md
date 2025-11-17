---
name: Meta-Skill Creator
description: 一个元技能，用于自动化从技术文档（如 Vercel AI SDK 文档）生成结构化 Claude Skills。支持用户协作设计蓝图，并通过子代理分析文档和渲染输出。遵循渐进式披露：先收集原材料，然后设计蓝图，构建并交付最终 SKILL 文件。
---

## 总体说明
这个技能是主编排者（Architect），用户唯一交互点。负责整个从文档到技能的自动化流程。渐进式披露：代理先读 description 决定触发；如果相关，加载主体指令；复杂步骤调用子代理。

## 工作流步骤

### 步骤 1: 收集原材料
- 新建临时目录 `temp-skills/temp-jsons` 把之后下面生成的 `all-analyses.json` 和 `meta-blueprint.json` 放入其中
- 根据用户提供文档目录（e.g., /vercel-ai-sdk/docs），如果没有找到相关目录，不要使用搜索工具，向先用户确认目录文件地址。
- 收集所有 `.md` 原始文档文件，使用 `cp` 复制到 `temp-skills/references` 目录
- 并行调用 <doc_analyzer_agent> 对 `references` 目录下的每个文档分析
- 生成 `all-analyses.json`（汇总 JSON，包括 summary, toc, key_apis 等）
- 将刚才的临时目录修改名称为：`[new-skill-name]/`（使用用户最终确认的技能名称）

### 步骤 2: 生成计划（Plan阶段）
- 展示 `all-analyses.json` 摘要给用户
- 基于分析结果，**生成多个技能计划选项**供用户选择：
  - **选项A：完整SDK技能** - 包含所有模块的大型综合技能
  - **选项B：核心功能技能** - 专注于最常用API的精简技能
  - **选项C：模块化技能集** - 按功能域拆分的多个小型技能
- 对每个选项提供：
  - 技能描述和预期用途
  - 预计文件结构
  - 主要功能覆盖范围
  - 推荐的使用场景
- 用户选择一个选项或要求混合定制

### 步骤 3: 设计蓝图（用户协作）
- 基于用户选择的计划，收集详细配置：
  - 技能名称（最终确认）
  - 技能描述（遵循第三人称标准）
  - 模块规划（"把 core.md 划分为 'core' 模块"）
  - 路由逻辑（"包含 useChat 的查询路由到 'ui' 模块"）
  - 跨模块模式（"定义一个 'Core + UI' 的完整流程"）
  - 渐进式披露策略（哪些内容放入references/）
- 生成 `meta-blueprint.json`（蓝图 JSON）
- 用户一般不会直接查看json文件，所以为用户生成摘要让用户确认，如果用户不满意，提示用户迭代
- 示例 `meta-blueprint.json`：
  ```json
  {
    "skill_name": "vercel-ai-sdk",
    "skill_description": "This skill should be used when users need to work with Vercel AI SDK for building AI-powered applications. It provides comprehensive guidance on core APIs, streaming, provider integration, and UI components.",
    "output_directory": "skills/vercel-ai-sdk/",
    "modules": {
      "core": { "source_docs": ["core.md"] },
      "ui": { "source_docs": ["ui.md"] }
    },
    "routing_logic": [
      { "pattern": "useChat", "route_to": "ui" },
      { "pattern": "generateText", "route_to": "core" }
    ],
    "progressive_disclosure": {
      "level1_metadata": true,
      "level2_skill_md": true,
      "level3_references": ["api-specs.md", "examples.md"]
    }
  }
  ```

### 步骤 4: 执行构建
- 调用 <skill_synthesizer_agent>，传递 `meta-blueprint.json`、`all-analyses.json` 和 `original_docs`
- 接收文件列表（[{path: 'SKILL.md', content: '...'} 等]）
- 在指定目录下创建精简技能结构：
  ```
  skills/[skill-name]/
  ├── SKILL.md
  └── references/
  ```

### 步骤 5: 交付内容
- 在 `[skill-name]/` 目录下生成精简文件：
  - 主 SKILL.md 文件（遵循渐进式披露和Anthropic标准）
- 仅在需要的时候以及用户明确同意时创建 `scripts/` 或 `assets/` 目录
- 验证输出（检查路由覆盖 key_apis、YAML格式等）
- 向用户报告生成结果和使用建议

## 子代理调用
- 使用 <doc_analyzer_agent> 分析单个文档。
- 使用 <skill_synthesizer_agent> 渲染最终输出。

## SKILL.md 生成标准（渐进式披露）

### Level 1: 元数据（始终加载）
- **name**: 使用kebab-case格式（如 vercel-ai-sdk）
- **description**: 第三人称描述，明确使用场景（"This skill should be used when..."）
- **核心原则**: 具体明确，避免模糊描述，确保轻量级上下文

### Level 2: SKILL.md 主体（技能触发时加载）
- **写作风格**: 使用祈使语气/不定式形式，避免第二人称
- **精简结构**:
  1. 技能概述和核心用途
  2. 主要工作流程和关键函数
  3. 资源引用指南（明确何时使用references/中的原始文档）
  4. 常见使用模式（仅最关键的）

### Level 3: 按需加载资源（references/）
- **核心原则**: 最终的SKILL.md文件中的资源文档都在工作目录的 `references/`，不重新生成内容，节省上下文
- **加载策略**: 在SKILL.md中明确指出何时引用原始文档

### YAML 前置元数据标准
```yaml
---
name: skill-name-here
description: This skill should be used when [specific scenario]. It provides [key functionality] for [user goal].
---
```

### 技能内容结构模板
```markdown
---
name: [skill-name-here]
description: This skill should be used when [specific scenario]. It provides [key functionality] for [user goal].
---

# [Skill Name]

## 核心功能
[2-3句话概括技能提供的主要功能]

## 使用时机
[复用description中的内容，保持一致性]

## 工作流程
1. [第一步具体操作]
2. [第二步具体操作]
3. [何时引用references/中的原始文档]

## 资源引用
- For detailed documentation: `references/[original-doc-name].md`
- 指导用户读取原始文档获取详细信息，避免重复生成内容
```

## 错误处理和验证
- **文档处理**: 如果文档过多，分批处理
- **蓝图验证**: 如果蓝图无效，迭代设计步骤
- **文件结构验证**: 确保生成的目录结构符合skill标准
- **YAML验证**: 确保frontmatter格式正确
- **渐进式披露验证**: 确保内容分层合理，避免SKILL.md过于臃肿

## 输出质量检查清单（优化版）
- [ ] 输出目录为 `[skill-name]/`
- [ ] SKILL.md包含正确的YAML frontmatter
- [ ] description使用第三人称（"This skill should be used when..."）
- [ ] 原始文档已使用 `cp` 复制到references/目录
- [ ] 精简目录结构（SKILL.md + references/ + temp-jsons/，避免生成不必要的其他文件）
- [ ] 所有资源引用路径指向原始文档
- [ ] 路由逻辑覆盖关键APIs
- [ ] 技能名称使用kebab-case格式
- [ ] 遵循渐进式披露原则（Level 1: ~100 words, Level 2: <5k words, Level 3: 按需加载）