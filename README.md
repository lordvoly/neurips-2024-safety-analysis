# Gaps in the Safety Net: A Large-Scale Empirical Analysis of NeurIPS 2024 Checklist Coverage

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Paper](https://img.shields.io/badge/paper-arXiv-red.svg)](YOUR_ARXIV_LINK_HERE)

This repository contains the code, data, and analysis for our systematic evaluation of safety checklist coverage at NeurIPS 2024. We analyze 1,033 accepted papers across 17 safety categories, identifying systematic gaps and providing evidence-based recommendations for improving conference peer review.

##  Overview

Conference safety checklists serve as critical gatekeepers for AI research, yet their effectiveness remains largely unvalidated. This work provides the **first large-scale empirical analysis** of checklist coverage, revealing three major gaps:

1. **93% non-response on transparency** (Question 14) despite widespread relevance
2. **147 surveillance-related papers** with no dedicated checklist coverage
3. **Limited mitigation strategies**: Only 12% of papers propose concrete risk reduction measures

We validate our keyword-based detection method through LLM comparison (Gemini 2.5 Flash, n=100), demonstrating strong agreement (>75%) for concrete categories while revealing limitations for abstract concepts.

## 📊 Key Findings

| Finding | Value | Impact |
|---------|-------|--------|
| Papers analyzed | 1,033 | Comprehensive NeurIPS 2024 coverage |
| Papers with safety concerns | 99.8% | Nearly universal detection |
| Average categories per paper | 5.9 | Multiple concurrent concerns |
| Q14 response rate | 7% | Systematic transparency gap |
| Surveillance papers | 147 (14.2%) | High-risk domain without coverage |
| Keyword-LLM agreement | 54.5% | Method validation |
| Misuse potential agreement | 93% | Excellent for concrete categories |
| Real-world harm agreement | 19% | Poor for abstract concepts |

##  Quick Start

### Prerequisites
```bash
python >= 3.8
pip >= 21.0
```

### Installation
```bash
# Clone repository
git clone https://github.com/YOUR-USERNAME/neurips-2024-safety-analysis.git
cd neurips-2024-safety-analysis

# Install dependencies
pip install -r requirements.txt

# Set up environment variables (for LLM comparison)
export GEMINI_API_KEY='your-api-key-here'
```

### Basic Usage
```python
from src.keyword_detection import SafetyDetector

# Initialize detector with 17-category taxonomy
detector = SafetyDetector(config_path='data/category_taxonomy.json')

# Analyze a paper
paper_text = "Your paper text here..."
results = detector.detect(paper_text)

# View detected categories
print(f"Detected categories: {results['categories']}")
print(f"Severity score: {results['severity_score']}")
print(f"Unmapped concerns: {results['unmapped']}")
```



## 🔧 Detailed Usage

### 1. Keyword-Based Detection

Analyze papers across 17 safety categories:
```python
from src.keyword_detection import SafetyDetector

detector = SafetyDetector()

# Single paper analysis
results = detector.detect_paper(paper_id, paper_text)

# Batch processing (1000+ papers)
results = detector.detect_batch(papers_dict, output_path='outputs/')
```

**Output format:**
```json
{
  "paper_id": "abc123",
  "categories_detected": ["misuse_potential", "fairness_violations"],
  "severity_score": 14,
  "category_count": 6,
  "unmapped_concerns": ["surveillance"],
  "checklist_mapping": {
    "Q9": ["fairness_violations"],
    "Q13": ["misuse_potential"]
  }
}
```

### 2. LLM Validation

Compare keyword detection with LLM analysis:
```python
from src.llm_comparison import LLMValidator

validator = LLMValidator(
    model='gemini-2.5-flash',
    api_key='your-key'
)

# Validate on sample
comparison = validator.compare(
    keyword_results=keyword_results,
    sample_size=100,
    stratified=True
)

# View metrics
print(f"Overall agreement: {comparison['agreement']:.2%}")
print(f"Precision: {comparison['precision']:.3f}")
print(f"Recall: {comparison['recall']:.3f}")
```

### 3. Reproduce Paper Figures
```bash
# Run visualization notebook
jupyter notebook notebooks/04_visualization.ipynb

# Or generate all figures via script
python src/generate_figures.py --output figures/
```

##  Safety Category Taxonomy

Our 17-category taxonomy covers:

| Category | Severity | NeurIPS Questions |
|----------|----------|-------------------|
| Data Privacy & Leakage | 3 | Q8, Q9 |
| Data Consent & Ethics | 3 | Q5, Q8 |
| Misuse Potential | 3 | Q13 |
| Real-World Harm | 3 | Q9, Q10, Q13 |
| Fairness Violations | 3 | Q9 |
| Bias Amplification | 3 | Q9 |
| Model Security | 3 | Q6, Q13 |
| Model Robustness | 2 | Q2, Q13 |
| Model Reliability | 2 | Q2, Q6 |
| Data Quality & Integrity | 2 | Q4, Q9 |
| Data Bias & Representation | 2 | Q9 |
| Deployment Readiness | 2 | Q6, Q11 |
| Evaluation Validity | 2 | Q1, Q3 |
| Model Interpretability | 1 | Q2 |
| Reproducibility Concerns | 1 | Q4, Q14, Q15 |
| Environmental Impact | 1 | Q2 |
| Resource Accessibility | 1 | Q2 |

**Severity weights:** 1=low, 2=medium, 3=high

##  Validation Results

Keyword vs. LLM comparison on 100-paper stratified sample:

| Category | Agreement | Precision | Recall | F1-Score |
|----------|-----------|-----------|--------|----------|
| Misuse Potential | 93% | 0.93 | 1.00 | 0.96 |
| Evaluation Validity | 94% | - | - | - |
| Data Quality | 85% | 0.50 | 0.07 | 0.12 |
| Fairness Violations | 79% | 0.83 | 1.00 | 0.79 |
| Real-World Harm | 19% | 0.87 | 0.14 | 0.24 |

**Key insight:** Keyword methods excel at concrete categories (misuse, evaluation) but struggle with abstract concepts (harm, bias amplification).

##  Recommendations

Based on our findings, we recommend:

1. **Make Q14 mandatory** with specific transparency prompts
2. **Add surveillance/high-risk application question** for 147+ papers
3. **Require mitigation strategies**, not just risk identification
4. **Expand checklist** for 5 systematically under-covered categories

##  Citation

If you use this code or data, please cite:
```bibtex
@article{yourname2024safety,
  title={Gaps in the Safety Net: A Large-Scale Empirical Analysis of NeurIPS 2024 Checklist Coverage},
  author={Aryan Batheja},
  ,
  year={2025}
}
```

##  License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

##  Acknowledgments

- NeurIPS 2024 for open access to paper data via OpenReview
- Google for Gemini API access
- AI safety research community for taxonomy foundations

##  Contact

For questions or collaboration:
- **Email:** t25ab24@abdn.ac.uk


##  Updates

- **2024-12-17:** Initial release

---

**⭐ Star this repo if you find it useful!**
