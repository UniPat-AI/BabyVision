# BabyVision

[![Blog](https://img.shields.io/badge/Blog-Read%20More-blue)](https://unipat.ai/blog/BabyVision) [![Leaderboard](https://img.shields.io/badge/Leaderboard-View%20Results-green)](https://unipat.ai/benchmarks/BabyVision) [![Paper](https://img.shields.io/badge/Paper-arXiv-red)](https://github.com/UniPat-AI/BabyVision/blob/main/BabyVision_Paper.pdf) [![HuggingFace](https://img.shields.io/badge/🤗%20BabyVision-Dataset-yellow)](https://huggingface.co/collections/UnipatAI/babyvision) 


State-of-the-art MLLMs achieve PhD-level language reasoning but struggle with visual tasks that 3-year-olds solve effortlessly. We introduce BabyVision, a benchmark revealing the infancy of AI vision. Read the [blog](https://unipat.ai/blog/BabyVision) first for better overall impression.

## Overview

BabyVision provides two evaluation tracks:

1. **MLLM Evaluation (Major)** (`babyvision_eval/`): Evaluate multimodal language models on visual reasoning tasks
2. **Generation Evaluation** (`babyvision_gen_eval/`): Evaluate image generation models on visual annotation tasks

Both tracks assess models across four visual reasoning categories:
- **Fine-grained Discrimination**: Finding different/same elements, shadows, patterns
- **Visual Tracking**: Solving mazes, connecting lines, metro maps
- **Spatial Perception**: 3D views, cube unfolding, paper folding, counting blocks
- **Visual Pattern Recognition**: Pattern completion tasks

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

See [babyvision_gen_eval/README.md](babyvision_gen_eval/README.md) for detailed documentation.

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
@article{babyvision2026,
  title={BabyVision: Visual Reasoning Beyond Language},
  year={2026}
}
```

## License

This project is released for research purposes.
