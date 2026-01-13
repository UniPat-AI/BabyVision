# BabyVision

<div align="center">
  <picture>
      <img src="./assets/baby_logo.png" width="30%">
  </picture>
</div>


<div align="center" style="line-height: 1;">

[![Leaderboard](https://img.shields.io/badge/Leaderboard-4285F4?style=for-the-badge&logo=google-chrome&logoColor=green)](https://unipat.ai/benchmarks/BabyVision)
[![GITHUB](https://img.shields.io/badge/Github-24292F?style=for-the-badge&logo=github&logoColor=white)](https://github.com/UniPat-AI/BabyVision)
[![Blog](https://img.shields.io/badge/Blog-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white)](https://unipat.ai/blog/BabyVision)
[![Paper](https://img.shields.io/badge/Paper-red?style=for-the-badge&logo=arxiv&logoColor=white)](https://github.com/UniPat-AI/BabyVision/blob/main/BabyVision_Paper.pdf)

</div>

> *Can MLLMs See Like a 3-Year-Old?*


State-of-the-art MLLMs achieve PhD-level language reasoning but struggle with visual tasks that 3-year-olds solve effortlessly. We introduce BabyVision, a benchmark revealing the infancy of AI vision. Read the [blog](https://unipat.ai/blog/BabyVision) first for better overall impression.

<div align="center">
  <picture>
      <img src="./assets/result.jpg" width="85%">
  </picture>
</div>


## Overview




BabyVision provides two evaluation tracks:

1. **MLLM Evaluation (Major)** ([./babyvision_eval/](./babyvision_eval/)): Evaluate multimodal language models on visual reasoning tasks.
2. **Generation Evaluation** ([./babyvision_gen_eval/](./babyvision_gen_eval/)): Evaluate image generation models on visual reasoning tasks.

Both tracks assess models across four visual reasoning categories:
- **Fine-grained Discrimination**: Finding different/same elements, shadows, patterns
- **Visual Tracking**: Solving mazes, connecting lines, metro maps
- **Spatial Perception**: 3D views, cube unfolding, paper folding, counting blocks
- **Visual Pattern Recognition**: Pattern completion tasks

<div align="center">
  <picture>
      <img src="./assets/full_table.jpg" width="85%">
  </picture>
</div>

## Leaderboard

[![Leaderboard](https://img.shields.io/badge/Leaderboard-4285F4?style=for-the-badge&logo=google-chrome&logoColor=green)](https://unipat.ai/benchmarks/BabyVision)

In the full and fine-grained evaluaiton, models' best performance is still far from human-level (94.1%). ​Across closed-source systems, ​Gemini3-Pro-Preview leads overall (49.7%)​, followed by ​GPT-5.2 (34.4%)​ and Doubao-Seed-1.8 (30.2%)​, with other models substantially lower (e.g., ​Qwen3-VL-Plus ​19.2%​, Grok-4 16.2%, ​Claude-4.5-Opus 14.2%​). 

<div align="center">
  <picture>
      <img src="./assets/radar1.png" width="85%">
  </picture>
</div>

## Repository Structure

```
BabyVision/
├── data/
│   ├── babyvision_data.zip       # MLLM evaluation data
│   ├── babyvision_gen_data.zip   # Generation evaluation data
│   └── mllm_results.zip          # MLLM Evaluation results
├── requirements.txt              # Python dependencies
│
├── babyvision_eval/              # MLLM Evaluation Package
│   ├── evaluate_model.py         # Main inference script
│   ├── compute_score.py          # Score computation
│   ├── run_inference.sh          # Shell wrapper
│   └── README.md                 # Detailed documentation
│
└── babyvision_gen_eval/          # Generation Evaluation Package
    ├── scripts/
    │   ├── inference.py          # Image generation inference
    │   ├── evaluate.py           # LLM-based evaluation
    │   └── summarize_results.py  # Result aggregation
    ├── inference.sh              # Shell wrapper
    ├── run_all_eval.sh           # Full evaluation pipeline
    └── README.md                 # Detailed documentation
```

## Quick Start

### Step 0: Extract Data

```bash
cd BabyVision

# For MLLM evaluation
unzip data/babyvision_data.zip -d data/

# For Generation evaluation
unzip data/babyvision_gen_data.zip -d data/
```
### Install

```
pip install -r requirements.txt
```

### Option A: MLLM Evaluation

Evaluate multimodal language models on visual reasoning tasks:

```bash
cd babyvision_eval

# Set API keys
export MODEL_API_KEY="your-model-api-key"
export MODEL_BASE_URL="https://openrouter.ai/api/v1"
export MODEL_NAME="google/gemini-3-flash-preview"
export JUDGE_API_KEY="your-judge-api-key"
export JUDGE_BASE_URL="https://openrouter.ai/api/v1"
export JUDGE_MODEL_NAME="openai/gpt-5.2" # or Qwen-Max 

# Run evaluation
bash run_inference.sh

# Compute scores
python compute_score.py results/model_results_run_*.json
```

See [babyvision_eval/README.md](babyvision_eval/README.md) for detailed documentation.

### Option B: Generation Evaluation

Evaluate image generation models on visual annotation tasks:

```bash
cd babyvision_gen_eval
pip install -r requirements.txt

# Set API key
export OPENROUTER_API_KEY="your-openrouter-key"

# Run inference
./inference.sh

# Run evaluation
./run_all_eval.sh

# View results
cat results/summary.txt
```

See [./babyvision_gen_eval/README.md](./babyvision_gen_eval/README.md) for detailed documentation.


## Evaluation Details

### MLLM Evaluation

- **Input**: Visual reasoning questions with images
- **Output**: Model answers in `\boxed{Answer}` format
- **Judging**: LLM judge compares model output to ground truth
- **Metrics**: Overall accuracy, type-wise accuracy, subtype-wise accuracy

### Generation Evaluation

- **Input**: Visual puzzles with annotation instructions
- **Output**: Annotated images (circles, lines, arrows marking answers)
- **Judging**: LLM compares generated images to ground truth images
- **Metrics**: Overall accuracy with mean/std across multiple rounds

## Configuration

Both evaluation packages support configuration via environment variables:

| Variable | MLLM Eval | Gen Eval | Description |
|----------|-----------|----------|-------------|
| `MODEL_API_KEY` | Required | - | API key for model |
| `JUDGE_API_KEY` | Required | - | API key for judge |
| `OPENROUTER_API_KEY` | - | Required | API key for OpenRouter |
| `MODEL_NAME` | Optional | Optional | Model to evaluate |
| `NUM_PASSES` / `ROUNDS` | Optional | Optional | Number of evaluation rounds |


## Scoring

Both tracks compute:
- **Overall Accuracy**: `correct / total_tasks`
- **Type-wise Accuracy**: Breakdown by task category
- **Subtype-wise Accuracy**: Detailed breakdown
- **Mean ± Std**: Statistics across multiple evaluation passes



## Citation

If you use this benchmark, please cite:

```bibtex
@misc{chen2026babyvisionvisualreasoninglanguage,
      title={BabyVision: Visual Reasoning Beyond Language}, 
      author={Liang Chen and Weichu Xie and Yiyan Liang and Hongfeng He and Hans Zhao and Zhibo Yang and Zhiqi Huang and Haoning Wu and Haoyu Lu and Y. charles and Yiping Bao and Yuantao Fan and Guopeng Li and Haiyang Shen and Xuanzhong Chen and Wendong Xu and Shuzheng Si and Zefan Cai and Wenhao Chai and Ziqi Huang and Fangfu Liu and Tianyu Liu and Baobao Chang and Xiaobo Hu and Kaiyuan Chen and Yixin Ren and Yang Liu and Yuan Gong and Kuan Li},
      year={2026},
      eprint={2601.06521},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2601.06521}, 
}
```

## License

This project is released for research purposes.
