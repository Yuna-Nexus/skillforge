# Article-Writing 技能泛化修改方案

## 任务目标

将 article-writing 技能从"药物递送系统专用"泛化为"通用学术论文写作工具"

## 核心需求

1. **主文档通用化**: 保留论文写作框架，移除具体研究方向表述
2. **研究方向可配置**: 设计可扩展的研究方向设定机制

## 修改方案状态

| 阶段 | 状态 | 说明 |
|------|------|------|
| 需求分析 | ✅ 完成 | 已分析所有相关文件 |
| 方案设计 | 🔄 进行中 | 正在制定详细方案 |
| 代码实现 | ⏳ 待开始 | 方案确认后执行 |
| 测试验证 | ⏳ 待开始 | 实现完成后测试 |

---

## 方案大纲

### Phase 1: SKILL.md 主文档泛化
- [ ] 移除"药物递送系统"等具体领域表述
- [ ] 修改身份定义为"通用学术写作专家"
- [ ] 保留所有写作流程和格式要求

### Phase 2: 研究方向配置系统设计
- [ ] 设计配置文件结构
- [ ] 实现配置加载模块（支持 /set-field 命令）
- [ ] 创建默认配置模板
- [ ] 实现用户自定义配置导入功能
- [ ] 创建内置配置文件（drug_delivery, clinical_pharmacy_llm, computer_science, quantitative_pharmacology）

### Phase 3: 模块文件泛化
- [ ] project_init.json 泛化
- [ ] reviewer_concerns.json 保留并转换为 configs/drug_delivery.json
- [ ] search_rules.json 示例泛化
- [ ] USAGE_GUIDE.md 添加多领域示例

### Phase 4: 测试与文档
- [ ] 创建测试用例
- [ ] 更新 README
- [ ] 编写配置指南

---

## 关键设计决策

### 1. 配置系统架构
- 采用 JSON 配置文件存储研究方向设定
- 支持用户自定义配置文件导入
- 保留原有 reviewer_concerns.json 并转换为配置文件
- /set-field 命令用于初始化时设置研究方向（不支持写作过程中动态切换）

### 2. 目录结构
```
article-writing/
├── SKILL.md                    # 主文档（泛化后）
├── configs/                    # 研究方向配置目录
│   ├── _schema.json           # JSON Schema
│   ├── default.json           # 通用默认配置
│   ├── drug_delivery.json    # 药物递送（保留原内容）
│   ├── clinical_pharmacy_llm.json  # 临床药学和大模型交叉学科
│   ├── computer_science.json # 计算机科学
│   └── quantitative_pharmacology.json  # 定量药理学
├── scripts/
│   ├── config_manager.py     # 配置管理模块
│   └── ...
├── templates/
│   └── ...
└── README.md
```

---

## 用户确认的决定

1. ✅ 保留原始药物递送配置作为子集
2. ✅ 不支持运行时动态切换（/set-field 仅用于初始化）
3. ✅ 支持用户自定义配置文件导入

---

*创建时间: 2026-03-03*
*最后更新: 2026-03-03*
