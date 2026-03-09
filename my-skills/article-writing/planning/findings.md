# Article-Writing 技能泛化 - 详细设计方案

## 1. SKILL.md 主文档修改方案

### 1.1 需要修改的位置

| 位置 | 原内容 | 修改后 |
|------|--------|--------|
| YAML description | `专注于广义药物递送系统领域` | `适用于多学科领域的学术研究论文` |
| 第11行 | `专注于广义药物递送系统领域` | `适用于多学科领域的学术研究论文` |
| 第37行身份 | `Nature Nanotechnology/Medicine 资深编辑 & 药物递送系统权威专家（25年经验）` | `Nature/Science/Cell 系列期刊资深编辑 & 学术写作专家（25年经验）` |

### 1.2 保留内容

- 所有写作流程协议
- 状态管理机制
- 引用格式要求
- 原子化文件管理
- 质量控制流程
- 图注生成协议
- SI 持久化协议

---

## 2. 研究方向配置系统设计

### 2.1 配置目录结构

```
article-writing/
├── configs/                          # 研究方向配置
│   ├── _schema.json                  # JSON Schema
│   ├── default.json                  # 通用默认配置
│   ├── drug_delivery.json           # 药物递送（保留原内容）
│   ├── clinical_pharmacy_llm.json    # 临床药学和大模型交叉学科
│   ├── computer_science.json        # 计算机科学
│   └── quantitative_pharmacology.json # 定量药理学
├── scripts/
│   ├── config_manager.py            # 配置管理模块
│   └── ...
└── templates/
    └── ...
```

### 2.2 配置文件 JSON Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["field_id", "field_name", "reviewer_concerns", "experiment_types"],
  "properties": {
    "field_id": {
      "type": "string",
      "description": "唯一标识符，如 drug_delivery, materials_science"
    },
    "field_name": {
      "type": "string",
      "description": "显示名称，如 药物递送系统、材料科学"
    },
    "field_name_en": {
      "type": "string",
      "description": "英文名称"
    },
    "target_journals": {
      "type": "array",
      "items": { "type": "string" },
      "description": "目标期刊列表"
    },
    "reviewer_concerns": {
      "type": "object",
      "properties": {
        "by_system": {
          "type": "object",
          "description": "按系统/类型的审稿人质疑",
          "additionalProperties": {
            "type": "array",
            "items": { "type": "string" }
          }
        },
        "by_experiment": {
          "type": "object",
          "description": "按实验类型的质疑",
          "additionalProperties": {
            "type": "array",
            "items": { "type": "string" }
          }
        },
        "mitigation_strategies": {
          "type": "object",
          "description": "缓解策略",
          "additionalProperties": {
            "type": "string"
          }
        }
      }
    },
    "experiment_types": {
      "type": "array",
      "items": { "type": "string" },
      "description": "支持的实验类型"
    },
    "literature_search_examples": {
      "type": "object",
      "properties": {
        "gap_statement": { "type": "string" },
        "innovation_claim": { "type": "string" },
        "methodology": { "type": "string" }
      }
    },
    "default_figure_requirements": {
      "type": "object",
      "properties": {
        "require_n_value": { "type": "boolean" },
        "require_scale_bar": { "type": "boolean" },
        "require_statistical_test": { "type": "boolean" }
      }
    }
  }
}
```

### 2.3 配置管理模块 (config_manager.py)

```python
#!/usr/bin/env python3
"""
研究方向配置管理器

功能：
- 加载/保存研究方向配置
- 配置切换与验证
- 默认配置管理
"""

import json
import os
from pathlib import Path
from typing import Dict, List, Optional

class FieldConfigManager:
    """研究方向配置管理器"""
    
    DEFAULT_CONFIG_DIR = Path(__file__).parent / "configs"
    DEFAULT_FIELD = "default"
    
    def __init__(self, config_dir: Optional[Path] = None):
        self.config_dir = config_dir or self.DEFAULT_CONFIG_DIR
        self._configs: Dict[str, dict] = {}
        self._current_field: Optional[str] = None
    
    def load_config(self, field_id: str) -> dict:
        """加载指定研究方向的配置"""
        if field_id in self._configs:
            return self._configs[field_id]
        
        config_path = self.config_dir / f"{field_id}.json"
        if not config_path.exists():
            raise FileNotFoundError(f"配置文件不存在: {field_id}")
        
        with open(config_path, 'r', encoding='utf-8') as f:
            config = json.load(f)
        
        self._configs[field_id] = config
        return config
    
    def get_current_config(self) -> dict:
        """获取当前激活的配置"""
        if not self._current_field:
            self._current_field = self.DEFAULT_FIELD
        return self.load_config(self._current_field)
    
    def switch_field(self, field_id: str) -> bool:
        """切换到指定研究方向"""
        try:
            config = self.load_config(field_id)
            self._current_field = field_id
            return True
        except FileNotFoundError:
            return False
    
    def list_available_fields(self) -> List[Dict[str, str]]:
        """列出所有可用的研究方向"""
        fields = []
        for config_file in self.config_dir.glob("*.json"):
            if config_file.name.startswith("_"):
                continue
            with open(config_file, 'r', encoding='utf-8') as f:
                config = json.load(f)
            fields.append({
                "id": config.get("field_id", config_file.stem),
                "name": config.get("field_name", config_file.stem),
                "name_en": config.get("field_name_en", "")
            })
        return fields
    
    def validate_config(self, config: dict) -> tuple[bool, List[str]]:
        """验证配置是否符合 Schema"""
        errors = []
        required = ["field_id", "field_name", "reviewer_concerns"]
        
        for field in required:
            if field not in config:
                errors.append(f"缺少必需字段: {field}")
        
        return len(errors) == 0, errors
    
    def create_config_from_template(self, field_id: str, field_name: str, output_path: Optional[Path] = None) -> Path:
        """从模板创建新配置"""
        template = {
            "field_id": field_id,
            "field_name": field_name,
            "field_name_en": field_id.replace("_", " ").title(),
            "target_journals": [],
            "reviewer_concerns": {
                "by_system": {},
                "by_experiment": {},
                "mitigation_strategies": {}
            },
            "experiment_types": ["Characterization", "Experimental"],
            "literature_search_examples": {
                "gap_statement": "当前研究领域的局限性...",
                "innovation_claim": "本研究的创新点...",
                "methodology": "方法学描述..."
            },
            "default_figure_requirements": {
                "require_n_value": True,
                "require_scale_bar": False,
                "require_statistical_test": True
            }
        }
        
        output_path = output_path or self.config_dir / f"{field_id}.json"
        with open(output_path, 'w', encoding='utf-8') as f:
            json.dump(template, f, ensure_ascii=False, indent=2)
        
        return output_path


# CLI 入口
if __name__ == "__main__":
    import argparse
    
    parser = argparse.ArgumentParser(description="研究方向配置管理器")
    parser.add_argument("action", choices=["list", "load", "create", "validate"],
                       help="操作类型")
    parser.add_argument("--field", help="研究方向ID")
    parser.add_argument("--name", help="研究方向名称（创建时使用）")
    parser.add_argument("--config-dir", type=Path, help="配置目录路径")
    
    args = parser.parse_args()
    manager = FieldConfigManager(args.config_dir)
    
    if args.action == "list":
        fields = manager.list_available_fields()
        print("可用研究方向:")
        for f in fields:
            print(f"  - {f['id']}: {f['name']}")
    
    elif args.action == "load" and args.field:
        config = manager.load_config(args.field)
        print(json.dumps(config, ensure_ascii=False, indent=2))
    
    elif args.action == "create" and args.field and args.name:
        path = manager.create_config_from_template(args.field, args.name)
        print(f"已创建配置文件: {path}")
    
    elif args.action == "validate" and args.field:
        config = manager.load_config(args.field)
        valid, errors = manager.validate_config(config)
        if valid:
            print("✓ 配置验证通过")
        else:
            print("✗ 配置验证失败:")
            for e in errors:
                print(f"  - {e}")
```

### 2.4 默认配置示例 (configs/default.json)

```json
{
  "field_id": "default",
  "field_name": "通用学术论文",
  "field_name_en": "General Academic",
  "target_journals": [
    "Nature",
    "Science", 
    "Cell",
    "PNAS",
    "Nature Communications",
    "Scientific Reports"
  ],
  "reviewer_concerns": {
    "by_system": {},
    "by_experiment": {
      "Methodology": [
        "实验方法是否详细到可重复？",
        "样本量/统计功效是否足够？",
        "对照组设置是否合理？"
      ],
      "Data_Analysis": [
        "统计方法选择是否恰当？",
        "数据呈现是否清晰？",
        "误差线/置信区间是否标注？"
      ],
      "Interpretation": [
        "结论是否有充分的数据支撑？",
        "是否讨论了局限性？",
        "是否与其他相关研究一致？"
      ]
    },
    "mitigation_strategies": {
      "methodology_reproducibility": "在Methods中详细描述实验步骤，包括试剂来源、仪器参数、操作条件等",
      "statistical_power": "提供样本量计算依据，说明统计检验方法",
      "limitations": "在Discussion中诚实讨论研究局限性"
    }
  },
  "experiment_types": [
    "Characterization",
    "Experimental",
    "Theoretical",
    "Computational",
    "Clinical",
    "Observational",
    "Review"
  ],
  "literature_search_examples": {
    "gap_statement": "{topic}领域目前的研究空白是...",
    "innovation_claim": "本研究首次提出...",
    "methodology": "{method}方法的原始文献..."
  },
  "default_figure_requirements": {
    "require_n_value": true,
    "require_scale_bar": false,
    "require_statistical_test": true
  },
  "word_limits": {
    "abstract": 250,
    "introduction": [800, 1500],
    "methods": [1000, 3000],
    "results": [2000, 4000],
    "discussion": [1500, 3000],
    "conclusion": [200, 500]
  }
}
```

### 2.5 药物递送配置 (configs/drug_delivery.json)

```json
{
  "field_id": "drug_delivery",
  "field_name": "药物递送系统",
  "field_name_en": "Drug Delivery System",
  "target_journals": [
    "Nature Nanotechnology",
    "Nature Materials",
    "Nature Biomedical Engineering",
    "Advanced Materials",
    "ACS Nano",
    "Journal of Controlled Release"
  ],
  "reviewer_concerns": {
    "by_system": {
      "Nanocarrier": [
        "EPR效应的临床转化争议",
        "长期蓄积毒性：纳米材料在肝脾的蓄积",
        "批次间稳定性：粒径、PDI、载药率一致性",
        "工业化放大：从实验室到GMP的CMC挑战",
        "生物降解性：材料的体内代谢途径",
        "免疫识别：MPS系统的快速清除问题"
      ],
      "Viral_Vector": [
        "免疫原性：预存抗体或重复给药引发的免疫反应",
        "基因组整合风险",
        "生产成本：病毒载体制备成本",
        "转导效率：特定细胞类型的靶向性",
        "包装容量：基因序列长度限制"
      ],
      "Cell_Therapy": [
        "离体扩增的遗传稳定性",
        "体内归巢效率",
        "细胞因子释放综合征（CRS）",
        "异质性问题：批次间差异",
        "冷链运输挑战"
      ],
      "Living_Bacteria": [
        "生物安全性：工程菌体内扩增控制",
        "肠道定植效率",
        "免疫耐受",
        "基因水平转移风险"
      ],
      "Exosome": [
        "纯度和标准化",
        "规模化生产成本",
        "载药效率",
        "靶向性",
        "储存稳定性"
      ]
    },
    "by_experiment": {
      "In_vivo_efficacy": [
        "动物模型是否能代表人体环境？",
        "给药剂量是否在临床可行范围？",
        "生存期实验终点是否合理？"
      ],
      "In_vitro_assays": [
        "细胞系passage数是否记录？",
        "2D培养 vs 3D培养的选择依据？",
        "血清浓度是否模拟体内条件？"
      ],
      "Characterization": [
        "DLS测定条件是否在生理条件下？",
        "TEM样品制备是否引入伪影？",
        "Zeta电位测定介质是否合适？"
      ]
    },
    "mitigation_strategies": {
      "EPR_controversy": "引用最新临床试验数据，承认EPR局限性",
      "batch_variability": "详细说明3个独立批次制备，CV<10%",
      "long_term_toxicity": "补充28天毒性评估数据"
    }
  },
  "experiment_types": [
    "Characterization",
    "In_vitro",
    "In_vivo",
    "Pharmacokinetics",
    "Biodistribution",
    "Toxicology"
  ],
  "literature_search_examples": {
    "gap_statement": "纳米药物肿瘤渗透性受限的问题...",
    "innovation_claim": "pH响应性电荷反转纳米粒子的创新设计...",
    "methodology": "动态光散射测量纳米粒径的标准协议..."
  },
  "default_figure_requirements": {
    "require_n_value": true,
    "require_scale_bar": true,
    "require_statistical_test": true
  }
}
```

### 2.4 临床药学和大模型交叉学科配置 (configs/clinical_pharmacy_llm.json)

```json
{
  "field_id": "clinical_pharmacy_llm",
  "field_name": "临床药学和大模型交叉学科",
  "field_name_en": "Clinical Pharmacy & LLM",
  "target_journals": [
    "Clinical Pharmacology & Therapeutics",
    "Journal of Clinical Pharmacology",
    "Drug Discovery Today",
    "npj Digital Medicine",
    "Journal of the American Medical Informatics Association"
  ],
  "reviewer_concerns": {
    "by_system": {
      "Pharmacokinetics": [
        "PK/PD模型的临床适用性验证",
        "种族差异对药物代谢的影响",
        "特殊人群（肝肾功能受损）的剂量调整依据"
      ],
      "LLM_Application": [
        "模型幻觉问题：生成内容的真实性验证",
        "训练数据偏见：是否代表目标人群",
        "可解释性：临床决策依据的透明度",
        "隐私保护：患者数据安全性"
      ],
      "Clinical_Trial": [
        "随机对照试验的样本量计算",
        "终点指标选择的临床意义",
        "亚组分析的预先规划"
      ]
    },
    "by_experiment": {
      "Retrospective_Analysis": [
        "混杂因素控制是否充分？",
        "缺失数据处理方法是否合理？",
        "因果推断方法是否恰当？"
      ],
      "Prospective_Study": [
        "干预措施是否标准化？",
        "盲法实施是否可行？",
        "随访脱落率是否在预期范围内？"
      ],
      "LLM_Evaluation": [
        "基准测试是否覆盖真实临床场景？",
        "人工评估的可靠性？",
        "与现有方法的比较是否公平？"
      ]
    },
    "mitigation_strategies": {
      "model_hallucination": "提供多源证据链，引用原始文献PMID或临床指南",
      "data_bias": "在Discussion中明确承认数据来源局限性，提供敏感性分析",
      "privacy_compliance": "使用脱敏数据，明确声明IRB批准"
    }
  },
  "experiment_types": [
    "Pharmacokinetic",
    "Pharmacodynamic",
    "Clinical_Trial",
    "Retrospective_Analysis",
    "LLM_Evaluation",
    "NLP_Analysis"
  ],
  "literature_search_examples": {
    "gap_statement": "当前LLM在临床决策支持中的局限性...",
    "innovation_claim": "首次将大模型应用于个体化用药剂量预测...",
    "methodology": "群体药代动力学模型构建的标准流程..."
  },
  "default_figure_requirements": {
    "require_n_value": true,
    "require_scale_bar": false,
    "require_statistical_test": true
  }
}
```

### 2.5 计算机科学配置 (configs/computer_science.json)

```json
{
  "field_id": "computer_science",
  "field_name": "计算机科学",
  "field_name_en": "Computer Science",
  "target_journals": [
    "Nature Machine Intelligence",
    "Science Advances",
    "Nature Communications",
    "ICML",
    "NeurIPS",
    "CVPR",
    "ACL"
  ],
  "reviewer_concerns": {
    "by_system": {
      "Algorithm": [
        "时间/空间复杂度分析是否充分？",
        "与现有SOTA方法的比较是否公平？",
        "理论保证与实际性能的一致性"
      ],
      "Dataset": [
        "数据来源和预处理流程是否可复现？",
        "训练/验证/测试集划分是否合理？",
        "数据偏见是否被充分讨论？"
      ],
      "Reproducibility": [
        "代码是否开源？",
        "超参数设置是否详细？",
        "随机种子是否固定？"
      ]
    },
    "by_experiment": {
      "Benchmark_Evaluation": [
        "基准测试是否覆盖主要任务？",
        "评估指标选择是否合理？",
        "统计显著性是否验证？"
      ],
      "Ablation_Study": [
        "各组件贡献是否被清晰分离？",
        "消融实验是否充分？"
      ],
      "Generalization": [
        "跨领域泛化能力是否验证？",
        "分布外数据表现如何？"
      ]
    },
    "mitigation_strategies": {
      "reproducibility": "提供完整代码仓库链接，包含依赖环境配置",
      "fair_comparison": "使用相同硬件、相同评估协议进行对比",
      "limitations": "诚实讨论方法的局限性和适用场景"
    }
  },
  "experiment_types": [
    "Algorithm_Design",
    "Benchmark_Evaluation",
    "Ablation_Study",
    "Theoretical_Analysis",
    "System_Implementation"
  ],
  "literature_search_examples": {
    "gap_statement": "当前深度学习模型在[任务]上的局限性...",
    "innovation_claim": "首次提出[新方法]解决[问题]...",
    "methodology": "标准训练流程和评估协议..."
  },
  "default_figure_requirements": {
    "require_n_value": false,
    "require_scale_bar": false,
    "require_statistical_test": true
  }
}
```

### 2.6 定量药理学配置 (configs/quantitative_pharmacology.json)

```json
{
  "field_id": "quantitative_pharmacology",
  "field_name": "定量药理学",
  "field_name_en": "Quantitative Pharmacology",
  "target_journals": [
    "Clinical Pharmacology & Therapeutics",
    "Journal of Pharmacokinetics and Pharmacodynamics",
    "CPT: Pharmacometrics & Systems Pharmacology",
    "Drug Metabolism and Pharmacokinetics",
    "British Journal of Clinical Pharmacology"
  ],
  "reviewer_concerns": {
    "by_system": {
      "PK_Model": [
        "房室模型选择的依据是否充分？",
        "参数估计的可靠性（基于RSE）",
        "模型诊断是否充分（残差、vpc）？"
      ],
      "PD_Model": [
        "药效学模型与疾病自然史的一致性",
        "生物标志物与临床终点的关联性",
        "效应室模型假设的合理性"
      ],
      "PBPK": [
        "器官清除率估算方法的依据",
        "组织分布假设的验证",
        "种属外推的科学性"
      ]
    },
    "by_experiment": {
      "Population_Analysis": [
        "协变量筛选方法的合理性",
        "Bootstrap/CI的稳定性",
        "模型验证策略是否充分？"
      ],
      "Simulation": [
        "模拟场景的临床相关性",
        "不确定性传播分析",
        "敏感性分析"
      ],
      "Dose_Finding": [
        "剂量选择依据的充分性",
        "目标暴露量定义的合理性",
        "疗效/安全性平衡的考量"
      ]
    },
    "mitigation_strategies": {
      "model_diagnostics": "提供完整的诊断图（DV vs IPRED, CWRES, VPC）",
      "parameter_precision": "报告RSE%，RSE>30%的参数需讨论",
      "clinical_relevance": "所有模拟需基于临床相关场景"
    }
  },
  "experiment_types": [
    "Population_PKPD",
    "PBPK_Modeling",
    "Exposure_Response",
    "Dose_Finding",
    "Simulation_Study"
  ],
  "literature_search_examples": {
    "gap_statement": "当前群体药代动力学模型在[药物]中的局限性...",
    "innovation_claim": "首次建立[新模型]预测[临床结局]...",
    "methodology": "NONMEM/Monolix/Merlion 建模标准流程..."
  },
  "default_figure_requirements": {
    "require_n_value": true,
    "require_scale_bar": false,
    "require_statistical_test": true
  }
}
```

---

## 3. templates/project_init.json 修改方案

### 3.1 修改内容

| 字段 | 原字段 | 修改后 | 说明 |
|------|--------|--------|------|
| target_journal | `"Nature Nanotechnology"` | `"Nature"` | 泛化为通用 |
| delivery_system_type | 专用字段 | research_field | 通用字段 |
| disease_model | 专用字段 | model_system | 通用字段 |

### 3.2 修改后的模板

```json
{
  "project_config_template": {
    "project_name": "",
    "research_field": "",
    "target_journal": "Nature",
    "model_system": "",
    "word_limits": {
      "abstract": 250,
      "introduction": [800, 1500],
      "main_text": [5000, 7000]
    },
    "created_date": "",
    "last_modified": "",
    "smart_snapshot_enabled": true,
    "field_config": "default"
  },
  "storyline_template": {
    "innovation_core": "",
    "main_hypothesis": "",
    "sections": { ... },
    "field_specific_requirements": {}
  },
  ...
}
```

---

## 4. 兼容性考虑

### 4.1 向后兼容

- 默认使用 `default.json` 配置
- 现有项目可通过设置 `field_config` 字段指定特定配置
- 旧版本项目自动使用通用配置

### 4.2 配置优先级

1. 项目级配置 (`project_config.json` 中的 `field_config`)
2. 用户级配置 (`~/.article-writing/config.json`)
3. 默认配置 (`configs/default.json`)

### 4.3 数据迁移

- 现有项目无需修改
- 首次加载时提示用户选择研究方向
- 可导出/导入配置

---

## 5. 实现步骤

### Step 1: 创建配置目录和基础配置

```
mkdir -p article-writing/configs
touch article-writing/configs/__init__.py
```

### Step 2: 创建配置文件

- `configs/default.json`
- `configs/drug_delivery.json`
- `configs/_schema.json`

### Step 3: 开发配置管理模块

- `scripts/config_manager.py`

### Step 4: 修改 SKILL.md

- 移除具体领域表述
- 添加配置系统说明
- 更新使用示例

### Step 5: 修改模板文件

- `templates/project_init.json`
- `templates/reviewer_concerns.json`
- `templates/search_rules.json`

### Step 6: 更新文档

- `README.md`
- `USAGE_GUIDE.md`

---

## 6. 待确认事项

1. **原有 reviewer_concerns.json 处理方式**
   - ✅ 方案A：保留并转换为 configs/drug_delivery.json

2. **配置动态切换机制**
   - ✅ 不支持运行时动态切换
   - ✅ /set-field 命令仅用于初始化时设置研究方向

3. **用户自定义配置支持**
   - ✅ 支持用户自定义配置文件导入
   - ✅ 路径：项目目录 configs/ 或用户目录 ~/.article-writing/configs/

4. **配置文件示例**
   - ✅ drug_delivery.json - 药物递送
   - ✅ clinical_pharmacy_llm.json - 临床药学和大模型交叉学科
   - ✅ computer_science.json - 计算机科学
   - ✅ quantitative_pharmacology.json - 定量药理学

---

*方案版本: 1.0*
*创建日期: 2026-03-03*
