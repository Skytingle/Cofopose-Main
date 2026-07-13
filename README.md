
# Cofopose: Conditional 2D Pose Estimation with Transformers

Official implementation of **"Cofopose: Conditional 2D Pose Estimation with Transformers"**, published in *Sensors* (2022). [[paper]](https://doi.org/10.3390/s22186821)

Cofopose is a two-stage approach to 2D human pose estimation. Rather than relying on heatmaps, it uses two transformers in sequence: a person-detection transformer that locates each person in the image, followed by a keypoint-detection transformer that regresses the body joints for each detected person. The design combines conditional cross-attention, a conditional DETR (DEtection TRansformer), and encoder-decoder blocks. Conditional cross-attention and a fine-tuned conditional DETR handle person detection, and encoder-decoders handle keypoint detection. The method was evaluated on the MS COCO and MPII benchmarks and improves on prior regression-based state-of-the-art results with less training.

> Note: verify the **Requirements** and **Usage** commands below against the actual code in this repository before relying on them.

## Method at a glance

- Two-stage: person detection transformer, then keypoint detection transformer.
- Conditional cross-attention plus fine-tuned conditional DETR for person detection.
- Transformer encoder-decoders for keypoint detection.
- 6 encoder layers, 6 decoder layers, 100 keypoint queries.
- Backbones used in the paper: ResNet-50, ResNet-101, ResNet-152, and HRNet-W32.

## Results

Headline numbers reported in the paper:

| Benchmark | Setting | Metric | Score |
|---|---|---|---|
| MS COCO (val) | HRNet-W32, 512x384 | AP | 74.2 |
| MS COCO (test-dev) | HRNet-W32, 512x384 | AP | 74.1 |
| MPII (val) | ResNet-101, 512x512, 75 epochs | PCKh@0.5 (Mean) | 90.1 |

On COCO val, Cofopose reaches 74.2 AP at 36 FPS, a competitive speed/accuracy trade-off. Full comparison tables and ablation studies are in the paper.

## Datasets

- **MS COCO**: used with the standard train/val/test-dev splits; pose annotations use 17 keypoints. Evaluated with mean Average Precision (AP) based on Object Keypoint Similarity (OKS).
- **MPII**: roughly 25k images and 40k people with 16 joint labels. Evaluated with PCKh@0.5.

Download each dataset from its official source and place it under a `data/` directory. The datasets are not included in this repository.


## Requirements

- Python 3.x
- PyTorch 1.10 with torchvision 0.11.1
- Additional packages listed in `requirements.txt`

Install everything with:

```
pip install -r requirements.txt
```

## Usage

All scripts are in the `tools/` directory, and experiment configuration files are in `experiments/`.

Training:

Evaluation:

> The exact argument names depend on this repository's config parser. Check the `argparse` section at the top of `tools/train.py` and `tools/test.py` and adjust the flags above to match.

## Repository structure

- `tools/` — training, testing, and tracing scripts (`train.py`, `test.py`, `trace.py`)
- `experiments/` — configuration files for the different backbones and datasets
- `lib/` — model, dataset, and utility code
- `models/` — model definitions or pretrained backbones
- `log/`, `output/` — training logs and results
