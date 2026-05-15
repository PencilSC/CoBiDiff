# CoBiDiff

This repository provides the research code for the paper:

**CoBiDiff: Anatomy-Aware Bidirectional Diffusion for Left Atrium Cross-Modal Augmentation**

CoBiDiff is an anatomy-aware bidirectional diffusion framework for cross-modal cardiac image augmentation between CT and MR images.

## Codebase Acknowledgement

This repository is developed based on the following open-source projects:

- DDIB: https://github.com/suxuann/ddib
- guided-diffusion: https://github.com/openai/guided-diffusion

The `ddib/` and `guided_diffusion/` directories include diffusion-related modules built upon DDIB and OpenAI guided-diffusion, with modifications for the CoBiDiff experimental pipeline.

We sincerely thank the authors of these repositories for their valuable open-source contributions.

## Introduction

This repository provides the code and scripts for the main experiments described in our manuscript.

The implementation is developed based on DDIB and OpenAI guided-diffusion, with modules for anatomy-aware CT-MR image augmentation, data processing, and translation.

The repository is organized to support academic research and reproducibility.

## Environment Requirements

The recommended environment is:

```text
Python 3.12
PyTorch 2.7.0
Torchvision 0.22.0
CUDA 12.8
```

Main dependencies include:

```text
blobfile==3.0.0
gdown==4.6.0
imageio==2.36.0
matplotlib==3.9.2
mpi4py
nibabel==5.3.2
numpy==1.26.4
opencv-python==4.8.1.78
pandas==2.1.4
pillow==11.0.0
PyYAML==6.0.2
scikit-image==0.24.0
scikit-learn==1.5.2
scipy==1.14.1
SimpleITK==2.5.0
tensorboard==2.19.0
tqdm==4.66.5
```

You may install the dependencies using:

```bash
pip install -r requirements.txt
```

or manually install the required packages according to your local CUDA and PyTorch versions.

## Project Structure

The repository is organized as follows:

```text
CoBiDiff/
├── ddib/                       # Bidirectional diffusion components
├── guided_diffusion/           # Diffusion model and sampling modules
├── scripts/                    # Scripts for running experiments
├── datasets/                   # Dataset loading and preprocessing utilities
├── checkpoints/                # Local directory for model checkpoints
├── results/                    # Local directory for generated samples and outputs
├── requirements.txt            # Python dependencies
└── README.md                   # Project documentation
```

The repository mainly contains:

- Diffusion-related implementation
- Experiment scripts and configuration files
- Dataset loading and preprocessing utilities
- Translation and sampling utilities

The `checkpoints/` directory is reserved for local model checkpoints, such as trained weights and training states. The `results/` directory is reserved for experimental outputs. These large files are not tracked in the repository by default.

## Usage

### 1. Clone the repository

```bash
git clone https://github.com/PencilSC/CoBiDiff.git
cd CoBiDiff
```

### 2. Create the environment

```bash
conda create -n cobidiff python=3.12
conda activate cobidiff
```

Install PyTorch according to your CUDA version. For example:

```bash
pip install torch==2.7.0 torchvision==0.22.0 --index-url https://download.pytorch.org/whl/cu128
```

For other CUDA versions, please install PyTorch according to the official PyTorch instructions.

Then install the remaining dependencies:

```bash
pip install -r requirements.txt
```

### 3. Prepare the dataset

Please download the public dataset from the official source and organize it according to the requirements of the data loading scripts.

An example data organization is shown below:

```text
datasets/image_train/
├── images/
│   ├── 0_ct/
│   └── 1_mr/
└── masks/
    ├── 0_ct/
    └── 1_mr/
```

This structure is only an example. Please modify the dataset path and organization according to the actual data loader.

### 4. Training

An example training command is:

```bash
MODEL_FLAGS="--class_cond True \
  --image_size 256 \
  --num_channels 128 \
  --num_res_blocks 2 \
  --num_head_channels 64 \
  --learn_sigma True \
  --use_scale_shift_norm True \
  --attention_resolutions 32,16,8 \
  --diffusion_steps 1000 \
  --noise_schedule linear \
  --rescale_learned_sigmas False \
  --rescale_timesteps False \
  --batch_size 8"

PYTHONPATH=. python scripts/image_train.py \
  --data_dir ./datasets/image_train \
  --lr 1e-4 \
  --save_interval 10000 \
  $MODEL_FLAGS
```

Please adjust the dataset path and hyperparameters according to your experimental setting.

### 5. Translation

An example CT-to-MR or MR-to-CT translation command is:

```bash
MODEL_FLAGS="--class_cond True \
  --image_size 256 \
  --num_channels 128 \
  --num_res_blocks 2 \
  --num_head_channels 64 \
  --in_channels 2 \
  --learn_sigma True \
  --use_scale_shift_norm True \
  --attention_resolutions 32,16,8 \
  --diffusion_steps 1000 \
  --noise_schedule linear \
  --batch_size 8"

PYTHONPATH=. python scripts/image_translation.py \
  --classifier_scale 5.0 \
  --source 0 \
  --target 1 \
  --val_dir ./datasets/image_val \
  $MODEL_FLAGS
```

Here, `--source` and `--target` indicate the source and target domains used for translation.

## Dataset

This study uses the public **MICCAI 2017 Multi-Modality Whole Heart Segmentation Challenge dataset**, also known as **MM-WHS 2017**.

Please download the dataset from the official public source.

The dataset is not redistributed in this repository. Users should follow the official dataset license, access policy, and citation requirements.

## Important Notes

This repository is intended for academic research and reproducibility.

Large files, local configurations, model checkpoints, generated samples, and intermediate experimental outputs are not tracked in this repository by default.

Please refer to the formally published version of the paper for the final experimental settings, results, and conclusions.

## Citation

If you use this code or find this work helpful, please cite our paper:

```bibtex
@misc{cobidiff2026,
  title  = {CoBiDiff: Anatomy-Aware Bidirectional Diffusion for Left Atrium Cross-Modal Augmentation},
  author = {Author Names},
  year   = {2026},
  note   = {Manuscript under review}
}
```

The citation information will be updated after the paper is formally published.

## Privacy and Security Statement

The repository is maintained to avoid personal information, local paths, private server addresses, and internal laboratory information.

Only publicly shareable code and example scripts are included in this repository.

## License

This repository is released for academic research purposes only.

Please also follow the licenses and usage terms of the original open-source repositories on which this implementation is based:

- DDIB
- guided-diffusion