---
name: research-publication-strategy
description: |
  Performs systematic research publication strategy analysis for scientific projects. 
  Use this skill when researchers need to analyze research directions, identify publication 
  strategies, conduct literature landscape analysis, identify research gaps, compare 
  related work, recommend target journals, design submission strategies, evaluate 
  innovation and workload, assess submission risks, and provide recommended paper structures.
  Perfect for AI/ML researchers, healthcare AI developers, medical informatics professionals,
  and clinical researchers preparing SCI paper submissions. Essential when working on:
  - AI agent systems, LLM applications in healthcare
  - Clinical decision support systems
  - Pharmacogenomics and precision medicine research
  - Medical informatics and health AI systems
  - Any research project requiring journal selection and submission planning
---

# Research Publication Strategy Analysis Skill

This skill performs comprehensive publication strategy analysis for scientific research projects. It helps researchers identify optimal publication paths, select target journals, and design submission strategies.

## Input Requirements

The skill accepts flexible research descriptions. Provide as much of the following as available:

| Field | Description |
|-------|-------------|
| research_project_name | Name or title of the research project |
| research_background | Context and motivation for the research |
| research_problem | Specific problem being addressed |
| research_method | Methodology used (algorithms, approaches) |
| system_architecture | Technical architecture if applicable |
| algorithm_description | Details of algorithms or models |
| datasets | Datasets used for training/evaluation |
| experiments | Experimental setup and design |
| evaluation_results | Quantitative/qualitative results |
| potential_paper_directions | Ideas for how the work could be framed |

**Note**: If information is incomplete, the skill will infer reasonable assumptions based on the provided details.

## Execution Modes

| Mode | Description |
|------|-------------|
| quick_analysis | Fast strategy evaluation with essential analysis |
| deep_research | Full literature analysis and comprehensive journal evaluation |

**Default mode**: deep_research

---

# Analysis Pipeline

## Step 1: Research Type Identification

Identify the research type based on the project description.

Possible categories:

| Research Type | Description |
|---------------|-------------|
| Method Paper | Novel algorithm or methodology development |
| System Paper | Engineering system or software platform |
| Application Paper | Applying existing methods to new domains |
| Translational Medical Research | Bench-to-bedside research |
| Clinical Decision Support System | Healthcare decision assistance systems |
| AI Agent System | Autonomous AI agents or LLM systems |
| Framework / Platform | General-purpose research frameworks |
| Survey / Review | Literature review or meta-analysis |

Output format:

| Research Type | Confidence | Reason |
|---------------|------------|--------|
| | | |

---

## Step 2: Publication Framing Analysis

Identify **2–4 possible publication strategies**. Each strategy represents a different paper narrative angle.

Possible framings based on research domain:

**AI/ML Domain:**
- AI Agent system architecture
- LLM reasoning method
- Machine learning algorithm
- Neural network architecture

**Healthcare Domain:**
- Clinical decision support framework
- Healthcare AI system evaluation
- Medical data analysis
- Clinical validation study

**Interdisciplinary:**
- Pharmacogenomics decision support
- Precision medicine platform
- AI-powered clinical system
- Health informatics framework

For each strategy provide:

| Field | Description |
|-------|-------------|
| Paper Theme | Core research narrative |
| Research Positioning | method / system / application |
| Target Audience | AI / medical / interdisciplinary |
| Key Contributions | Major innovations |
| Required Experiments | Experiments needed to support this framing |

---

## Step 3: Literature Search Strategy

Generate a systematic literature search strategy.

**Recommended databases:**
- PubMed
- Web of Science
- Scopus
- Google Scholar
- IEEE Xplore
- ACM Digital Library

**Suggested query structures by domain:**

For AI + Healthcare:
```
("large language model" OR LLM OR "AI agent" OR "deep learning")
AND
("clinical decision support" OR "precision medicine" OR "personalized medicine" OR "healthcare")
AND
(system OR framework OR architecture OR evaluation)
```

For Pharmacogenomics:
```
("pharmacogenomics" OR "pharmacogenetics" OR "precision dosing")
AND
("clinical decision support" OR "drug therapy optimization")
AND
(system OR algorithm OR framework)
```

For Clinical Pharmacology:
```
("pharmacotherapy" OR "clinical pharmacology" OR "rational drug use")
AND
("decision support" OR "treatment optimization")
AND
(AI OR machine learning OR algorithm)
```

Output:

| Database | Suggested Query | Purpose |
|---------|-----------------|---------|
| PubMed | [generated query] | Medical literature |
| IEEE Xplore | [AI-focused query] | Technical/AI papers |
| Web of Science | [cross-disciplinary] | Citation analysis |

---

## Step 4: Literature Landscape Analysis

Analyze the research field over the past **5 years**.

Key elements to identify:

1. **Major research topics** - Hot areas and trending themes
2. **Publication trend** - Growth patterns by year
3. **Key research groups** - Leading institutions and labs
4. **Typical methods** - Common approaches in the field
5. **Dominant evaluation datasets** - Standard benchmarks
6. **Emerging research directions** - New frontiers

Output summary tables where possible:

| Topic | Papers (5yr) | Trend | Key Methods |
|-------|--------------|-------|-------------|
| | | | |

---

## Step 5: Research Gap Identification

Identify **important research gaps** in the current literature.

Possible gap categories:

| Gap Type | Description |
|----------|-------------|
| Methodological limitations | Technical shortcomings in existing approaches |
| Lack of real-world validation | Limited clinical or practical deployment studies |
| Limited datasets | Scarcity of benchmark datasets |
| Missing system integration | Lack of end-to-end systems |
| Poor interpretability | Black-box models without explanations |
| Missing explainability | No decision rationale provided |

Output:

| Gap Type | Description | How This Research Addresses It |
|----------|-------------|-------------------------------|
| | | |

---

## Step 6: Related Work Comparison

Construct a **comparison matrix** of representative related papers.

Include **5–10 representative studies** when possible, focusing on:
- Clinical decision support systems
- AI in healthcare
- Pharmacogenomics systems
- LLM medical reasoning systems

Output format:

| Paper | Method | Dataset | Contribution | Limitation |
|-------|--------|---------|--------------|------------|
| | | | | |

---

## Step 7: Journal Selection Strategy

Identify suitable journals based on research domain.

**Target journal categories:**

Medical Informatics / Healthcare AI:
- Journal of Biomedical Informatics
- JAMIA (Journal of the American Medical Informatics Association)
- npj Digital Medicine
- Artificial Intelligence in Medicine
- IEEE Journal of Biomedical and Health Informatics

Clinical Pharmacology:
- Clinical Pharmacology & Therapeutics
- British Journal of Clinical Pharmacology
- Pharmacotherapy
- Journal of Clinical Pharmacology

AI / Computer Science:
- IEEE TMI (Transactions on Medical Imaging)
- Knowledge-Based Systems
- Expert Systems with Applications
- Artificial Intelligence

Construct a **journal evaluation matrix**:

| Journal | Impact Factor | JCR Quartile | Field | Acceptance Difficulty | Review Speed | Similar Papers |
|---------|---------------|--------------|-------|----------------------|--------------|----------------|
| | | | | | | |

**Analysis factors:**
- Journal scope fit
- AI/healthcare acceptance track record
- Recent similar publications
- Review timeline alignment

---

## Step 8: Submission Strategy Design

Create **at least three complete submission plans**.

Each plan must include:

| Field | Description |
|-------|-------------|
| Paper Theme | |
| Target Journal | |
| Research Positioning | |
| Key Innovation | |
| Required Experiments | |
| Expected Difficulty | |
| Estimated Acceptance Probability | |

---

## Step 9: Submission Priority Ranking

Rank the submission strategies based on:

| Criteria | Weight |
|----------|--------|
| Topic–journal fit | High |
| Innovation level | High |
| Experimental completeness | High |
| Acceptance probability | Medium |
| Journal influence | Medium |
| Publication timeline | Medium |

Output:

| Rank | Strategy | Target Journal | Recommendation Reason |
|------|----------|----------------|----------------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

## Step 10: Innovation and Workload Evaluation

### Research Workload Assessment

Evaluate:

| Dimension | Rating | Notes |
|-----------|--------|-------|
| Dataset scale | | |
| Algorithm complexity | | |
| System engineering effort | | |
| Experimental validation depth | | |
| Clinical validation level | | |

### Innovation Assessment

Evaluate innovation in:

| Dimension | Rating | Notes |
|-----------|--------|-------|
| Methodology | | |
| System architecture | | |
| Application scenario | | |
| Interdisciplinary integration | | |

---

## Step 11: Submission Risk Assessment

Identify potential risks that could cause **desk rejection or reviewer criticism**.

Common risks:

| Risk | Description | Suggested Solution |
|------|-------------|-------------------|
| Insufficient experiments | Not enough evaluation | Expand benchmarks |
| Lack of baseline comparisons | No SOTA comparison | Add comparison experiments |
| Weak clinical validation | Limited real-world evidence | Increase clinical data |
| Unclear novelty | Contribution not well motivated | Strengthen introduction |
| Missing theoretical justification | No theoretical grounding | Add theoretical analysis |

Output full risk table with mitigation strategies.

---

## Step 12: Suggested Paper Structure

Recommend an optimized **paper outline** based on the selected strategy.

### AI Method Paper Structure

```
1 Introduction
   - Problem definition
   - Research gap
   - Contributions
   
2 Related Work
   - Method surveys
   - Comparative analysis
   
3 Method
   - Framework overview
   - Technical details
   - Theoretical analysis
   
4 Experiments
   - Datasets
   - Baselines
   - Results
   - Ablation studies
   
5 Discussion
   - Limitations
   - Broader impact
   
6 Conclusion
```

### AI System Paper Structure

```
1 Introduction
2 Related Work
3 System Architecture
4 Method Components
5 Evaluation
6 Case Study
7 Discussion
8 Conclusion
```

### Clinical Decision Support Paper Structure

```
1 Introduction
2 Related Work
3 System Design
4 Expert Policy / Decision Logic
5 Clinical Evaluation
6 Case Studies
7 Discussion
8 Conclusion
```

---

## Step 13: Potential Research Groups and Reviewers (Optional)

Identify important research groups in the field.

| Research Group | Institution | Research Focus |
|----------------|-------------|----------------|
| | | |

This helps understand the **review ecosystem** and potential reviewers.

---

# Output Requirements

The skill outputs a **structured Markdown report** with the following sections:

```
# SCI Publication Strategy Analysis Report

## 1 Research Overview
[Project summary and key details]

## 2 Research Type Identification
[Research type with confidence score]

## 3 Publication Framing Analysis
[2-4 framing strategies with details]

## 4 Literature Search Strategy
[Database-specific queries]

## 5 Literature Landscape Analysis
[Field overview and trends]

## 6 Research Gap Identification
[Identified gaps and solutions]

## 7 Related Work Comparison
[Comparison matrix]

## 8 Journal Selection Strategy
[Journal evaluation matrix]

## 9 Submission Strategy Design
[3+ complete submission plans]

## 10 Submission Priority Ranking
[Ranked strategies]

## 11 Innovation and Workload Evaluation
[Structured evaluation]

## 12 Submission Risk Assessment
[Risk matrix with mitigations]

## 13 Suggested Paper Structure
[Recommended outline]

## 14 Recommended Submission Plan
[Final recommendation]
```

---

# Behavior Requirements

- Produce **clear analytical reasoning** with evidence-based justifications
- Prioritize **structured tables** for all comparative analyses
- Focus on **decision-support insights** that enable actionable choices
- Remain **domain adaptable** across AI, healthcare, and interdisciplinary research
- Be reusable for **future research projects**
- Avoid vague descriptions—provide specific, quantified recommendations

---

# Usage Examples

**Trigger phrases:**
- "帮我分析这个研究项目的发表策略"
- "分析我的AI医疗研究如何选择期刊"
- "设计一个临床决策支持系统的投稿计划"
- "评估这项研究的创新性和工作量"
- "帮我找到相关的期刊和潜在审稿人"

**Input examples:**
- Project name: "LLM-based Clinical Decision Support System"
- Research problem: "Need to diagnose rare diseases using limited patient data"
- Method: "Multi-modal transformer with knowledge graph integration"
- Results: "Achieved 92% accuracy on MIMIC-III, outperforming baselines by 5%"

---

# Tips for Best Results

1. **Provide detailed research description** - More details lead to better analysis
2. **Specify target journal preferences** - If you have preferences, include them
3. **Indicate timeline constraints** - Publication urgency affects strategy
4. **Mention competitive work** - Known related work helps gap analysis
5. **State any preliminary rejections** - Past feedback informs improvements

The skill will generate a comprehensive analysis report that directly supports SCI paper submission decisions.
