# (ICCV 2025 Highlight) CMP: Cross-Modal Pose-aware framework for Text-based Person Anomaly Search


**[Beyond Walking: A Large-Scale Image-Text Benchmark 
for Text-based Person Anomaly Search](https://arxiv.org/abs/2411.17776). 
Shuyu Yang, Yaxiong Wang, Li Zhu, Zhedong Zheng. arXiv 2024.**


- Jun 2025: Our paper has been accepted to **ICCV 2025**
- Mar 2025: Release attribute annotation of PAB
- Dec 2024: Release official PyTorch implementation, CMP checkpoints, and PAB dataset
- Nov 2024: Release preprint in [arXiv](https://arxiv.org/abs/2411.17776)


We introduce the **task** of **Text-based Person Anomaly Search**, 
which aims to locate pedestrians engaged in both routine and anomalous 
activities using natural language descriptions. Given the lack of a 
**dataset** in this field, we construct the **P**edestrian **A**nomaly 
**B**ehavior (**PAB**) benchmark, featuring **1, 013, 605** synthesized 
and **1, 978** real-world image-text pairs with a broad spectrum of 
actions and anomalies.

This is the comparison of our proposed task, i.e., Text-based Person Anomaly Search (right) vs. Traditional Text-Based Person
Search (left). 
<div align="center"><img src="assets/example.jpg" width="900"></div>


We propose a **C**ross-**M**odal **P**ose-aware (**CMP**) framework that 
integrates human pose patterns with identity-based hard negative pair 
sampling to enhance the discrimination between normal and anomalous behaviors. 
This framework leverages structural information from human poses 
to improve the understanding of pedestrian activities, 
leading to better retrieval performance.

Extensive experiments on the PAB benchmark show that synthetic training data 
effectively facilitates fine-grained behavior retrieval in real-world 
test sets. Our pose-aware method arrives at 84.93% recall@1 accuracy, 
surpassing other competitive methods.
More details can be found at our paper: 
[Beyond Walking: A Large-Scale Image-Text Benchmark 
for Text-based Person Anomaly Search](https://arxiv.org/abs/2411.17776)



## PAB
PAB leverages generative models to generate a large-scale dataset including 
1𝑀 image-text pairs. Each image-text pair in PAB is annotated with 
action and scene attribute, indicating that PAB is not only effective 
for Text-based Person Anomaly Search, but also supports 
future attribute recognition tasks like action or scene classification.
The dataset is released at 
[OneDrive](https://1drv.ms/f/c/afc02d7952f9b34d/Epb3qCEwsMJOjYIx-sMm_rkBbZfyiD8I-bRmLp0X-rT1vQ?e=7gyGco) 
& [Baidu Yun](https://pan.baidu.com/s/1gqY6DuTL-EStXlH0dz05ng) [mdjb].

**Note that PAB can only be used for research, any commercial usage is forbidden.**

This is the comparison between PAB and existing text-based pedestrian search and video anomaly detection datasets in terms of data quality and
quantity. 
<div align="center"><img src="assets/comparison.JPG" width="900"></div>
These are dataset properties and examples of PAB.
<div align="center"><img src="assets/dataset.jpg" width="900"></div>

Annotation format:

```
{"image": "train/imgs_0/goal/0.jpg", 
"caption": "The image shows a band performing on stage under a large tent...", 
"image_id": "0_0", 
"hard_i": "imgs_0/full/0.jpg", 
"hard_c": "The image shows a band performing under a large white tent...", 
"hard_i_id": "0_8954", 
"source_id": "1_0", 
"source_caption": "band was performing under a big tent", 
"normal": "Performing", 
"scene": "outdoor concert"}
...
{"image": "train/imgs_0/full/6667.jpg", 
"caption": "The image shows a person running on a grassy field...",
"image_id": "0_13630", 
"hard_i": "imgs_0/goal/6736.jpg", 
"hard_c": "The image shows a person running on a grassy field.",
"hard_i_id": "0_5077", 
"source_id": "3617_2", 
"source_caption": "...he kept falling to the ground", 
"anomaly": "Falling", 
"scene": "Lawn"
...
```

## Models and Weights

This is an overview of our proposed Cross-Modal Pose-aware (CMP) framework. 
<div align="center"><img src="assets/framework.jpg" width="900"></div>

The checkpoints `cmp.pth` and training log have been released at
[Google Drive](https://drive.google.com/drive/folders/1qdOIwyXD72gbccqGGRxvqg4gWEVI2ceA?usp=sharing)
& [Baidu Yun](https://pan.baidu.com/s/18x-kwybNq6Jdfe5s2U4Tgw?pwd=d4ba) [d4ba].


## Usage

### Install Requirements

We use 4 NVIDIA GeForce RTX 3090 GPUs (24G) for training and evaluation.

Clone the repo:
```sh
git clone https://github.com/Shuyu-XJTU/CMP.git
cd CMP
```
Create conda environment and install dependencies:

```sh
conda create -n cmp python=3.10
conda activate cmp
# Ensure torch >= 2.0.0 and install torch based on CUDA Version
# For example, if CUDA Version is 11.8, install torch 2.2.0:
pip install torch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 --index-url https://download.pytorch.org/whl/cu118
pip3 install -r requirements.txt
```

For the first time you use wordnet
```
python
>>> import nltk
>>> nltk.download('wordnet')
```

### Parameter Initialization

Download pre-trained models for parameter initialization:

Initializing parameters from [X-VLM (16M)](https://github.com/zengyan-97/x-vlm): 
[16m_base_model_state_step_199999.th](https://drive.google.com/file/d/1iXgITaSbQ1oGPPvGaV0Hlae4QiJG5gx0/view?usp=sharing)

Text tokenizer/encoder: [bert-base-uncased](https://huggingface.co/bert-base-uncased/tree/main)

Image encoder: [swin-transformer-base](https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_base_patch4_window7_224_22k.pth)

Organize `checkpoint` folder as follows:

```
|-- checkpoint/
|    |-- cmp.pth
|    |-- 16m_base_model_state_step_199999.th
|    |-- bert-base-uncased/
|    |-- swin_base_patch4_window7_224_22k.pth
```

### Datasets Prepare

And organize PAB in `data/PAB` folder as follows:

```
|-- PAB/
|    |-- annotation/
|        |-- train/
|            |-- attr_0.json
|            |-- ...
|        |-- test/
|            |-- attr.json
|            |-- ucc.json
|        |-- source_caption.json
|    |-- train/
|        |-- imgs_0/
|            |-- goal/
|                |-- 0.jpg
|                |-- ...
|            |-- wentrong/
|            |-- full/
|        |-- imgs_1/
|        |-- ...
|    |-- test/
|        |-- 0.jpg
|        |-- ...
|    |-- ucc/
|    |-- pose/
|        |-- train/
|            |-- imgs_0/
|            |-- ...
|        |-- test/
|        |-- ucc/
```

### Training
We train our CMP using PAB as follows：

```
python3 run.py --task "cmp" --dist "f4" --output_dir "output/cmp"
```

### Evaluation

```
python3 run.py --task "cmp" --evaluate --dist "f4" --output_dir "output/cmp_eval" --checkpoint "checkpoint/cmp.pth"
```

## Reference
If you use PAB or CMP in your research, please cite it by the following BibTeX entry:

```
@inproceedings{yang2025beyond,
  title={Beyond Walking: A Large-Scale Image-Text Benchmark for Text-based Person Anomaly Search},
  author={Yang, Shuyu and Wang, Yaxiong and Zhu, Li and Zheng, Zhedong},
  booktitle={ICCV},
  year={2025}
}

```

## 📊 Leaderboard

We maintain a leaderboard of Text-Based Person Retrieval methods.

👉 [View Leaderboard](leaderboard/README.md)


## 🔗 Related Projects

<p align="center"><i>Explore our ecosystem for Object Re-Identification 🔍</i></p >

<div align="center">
  <table>
    <tr>
      <td align="center" colspan="3">
        <a href=" ">
          <h3>⛹️</h3>
          <b>Person re-ID Baseline (PyTorch)</b>
        </a >
        <br><sub>A <b>Tiny, Friendly & Strong</b> PyTorch Baseline for Person / Vehicle Re-ID<br>with Hands-on Tutorial · The Community Standard since 2017</sub>
        <br><br>
        <a href="https://github.com/layumi/Person_reID_baseline_pytorch"><img src="https://img.shields.io/github/stars/layumi/Person_reID_baseline_pytorch.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
    </tr>
    <tr>
      <td align="center" width="33%">
        <a href="https://github.com/layumi/3D-Magic-Mirror">
          <h3>🪞</h3>
          <b>3D Magic Mirror</b>
        </a >
        <br><sub>Clothing Reconstruction from a Single Image<br>via a Causal Perspective · npj AI'26</sub>
        <br><br>
        <a href="https://github.com/layumi/3D-Magic-Mirror">< img src="https://img.shields.io/github/stars/layumi/3D-Magic-Mirror.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
      <td align="center" width="33%">
        <a href="https://github.com/NVlabs/DG-Net">
          <h3>✨</h3>
          <b>DG-Net</b>
        </a >
        <br><sub>Joint Generation + Re-ID Learning<br>CVPR'19 Oral · NVIDIA</sub>
        <br><br>
        <a href="https://github.com/NVlabs/DG-Net">< img src="https://img.shields.io/github/stars/NVlabs/DG-Net.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
      <td align="center" width="33%">
        <a href="https://github.com/layumi/Person-reID_GAN">
          <h3>🎨</h3>
          <b>Person re-ID GAN</b>
        </a >
        <br><sub>GAN-based Augmentation (LSRO)<br>ICCV'17</sub>
        <br><br>
        <a href="https://github.com/layumi/Person-reID_GAN">< img src="https://img.shields.io/github/stars/layumi/Person-reID_GAN.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
    </tr>
    <tr>
      <td align="center" width="33%">
        <a href="https://github.com/layumi/Image-Text-Embedding">
          <h3>📝</h3>
          <b>Language Person Search</b>
        </a >
        <br><sub>Text-based Person Retrieval<br>Dual-Path Embedding</sub>
        <br><br>
        <a href="https://github.com/layumi/Image-Text-Embedding">< img src="https://img.shields.io/github/stars/layumi/Image-Text-Embedding.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
      <td align="center" width="33%">
        <a href="https://github.com/Shuyu-XJTU/APTM">
          <h3>🏷️</h3>
          <b>APTM</b>
        </a >
        <br><sub>Attribute Prompt Learning &amp; Text Matching<br>MALS Benchmark (1.5M pairs) · ACM MM'23</sub>
        <br><br>
        <a href="https://github.com/Shuyu-XJTU/APTM">< img src="https://img.shields.io/github/stars/Shuyu-XJTU/APTM.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
      <td align="center" width="33%">
        <a href="https://github.com/Shuyu-XJTU/CMP">
          <h3>🚨</h3>
          <b>CMP</b>
        </a >
        <br><sub>Text-based Person <b>Anomaly</b> Search<br>PAB Benchmark (1M pairs) · ICCV'25 <b>Highlight</b></sub>
        <br><br>
        <a href="https://github.com/Shuyu-XJTU/CMP">< img src="https://img.shields.io/github/stars/Shuyu-XJTU/CMP.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
    </tr>
    <tr>
      <td align="center" width="33%">
        <a href="https://github.com/layumi/person-reid-3d">
          <h3>🧊</h3>
          <b>3D Person re-ID</b>
        </a >
        <br><sub>Parameter-Efficient Re-ID<br>in the 3D Space (OG-Net)</sub>
        <br><br>
        <a href="https://github.com/layumi/person-reid-3d">< img src="https://img.shields.io/github/stars/layumi/person-reid-3d.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
      <td align="center" width="33%">
        <a href="https://github.com/layumi/Pedestrian_Alignment">
          <h3>🚶</h3>
          <b>Pedestrian Alignment</b>
        </a >
        <br><sub>Pedestrian Alignment Network (PAN)<br>for Robust Re-ID</sub>
        <br><br>
        <a href="https://github.com/layumi/Pedestrian_Alignment">< img src="https://img.shields.io/github/stars/layumi/Pedestrian_Alignment.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
      <td align="center" width="33%">
        <a href="https://github.com/layumi/2016_person_re-ID">
          <h3>🔁</h3>
          <b>2-Stream Person re-ID</b>
        </a >
        <br><sub>Verification + Identification<br>Baseline</sub>
        <br><br>
        <a href="https://github.com/layumi/2016_person_re-ID">< img src="https://img.shields.io/github/stars/layumi/2016_person_re-ID.svg?style=social&label=Star" alt="GitHub stars"></a >
      </td>
    </tr>
  </table>
</div>


---

<p align="center">
  ⭐ If you find our projects helpful, a <b>star</b> is the best support! ⭐
</p >
