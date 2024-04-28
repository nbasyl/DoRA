<h1 align="center">
    <p>DoRA: Weight-Decomposed Low-Rank Adaptation</p>
</h1>

<h1 align="center"> 
    <img src="./dora.png" width="600">
</h1>

# This repo is now deprecated, please visit [NVlabs/DoRA](https://github.com/NVlabs/DoRA) instead!!

## DoRA: Weight-Decomposed Low-Rank Adaptation

Shih-Yang Liu, Chien-Yi Wang, Hongxu Yin, Pavlo Molchanov, Yu-Chiang Frank Wang, Kwang-Ting Cheng, Min-Hung Chen

Paper: https://arxiv.org/abs/2402.09353

Project page: https://nbasyl.github.io/DoRA-project-page/

DoRA decomposes the pre-trained weight into two components, magnitude and direction, for fine-tuning, specifically employing LoRA for directional updates to efficiently minimize the number of trainable parameters. By employing DoRA, we enhance both the learning capacity and training stability of LoRA while avoiding any additional inference overhead. DoRA consistently outperforms LoRA on fine-tuning LLaMA, LLaVA, and VL-BART on various downstream tasks, such as commonsense reasoning, visual instruction tuning, and image/video-text understanding.

## Quick Start and some tricks regarding finetuing with DoRA
### HuggingFace PEFT
DoRA is now supported by the Huggingface PEFT package. You can install the PEFT package using
```bash
pip install git+https://github.com/huggingface/peft.git -q
```

After PEFT is installed, you can simply set the `use_dora` argument of `LoraConfig()` to `True` for applying DoRA.

An example could be as follows:
```bash
from peft import LoraConfig

# Initialize DoRA configuration
config = (
    use_dora=True, ...
)
```
Please refer to the official [documentation](https://huggingface.co/docs/peft/en/developer_guides/lora#weight-decomposed-low-rank-adaptation-dora) for more details.

### DoRA hyperparameters settings
> [!NOTE]
> 💡 While fine-tuning with DoRA, by utilizing the configuration of LoRA can already achieve better results most of the time, achieving optimal performance compared to LoRA still requires adjustments to the hyperparameters. 

> We suggest starting with a slightly lower learning rate than that of LoRA, and users may also experiment with varying lora dropout ratios.

> User may also start with half of the rank of the LoRA configuration which oftentime can already results in comparable or even superior accuracy compared to that of LoRA.

## Contact
Shih-Yang Liu: [shihyangl@nvidia.com](shihyangl@nvidia.com) or [sliuau@connect.ust.hk](sliuau@connect.ust.hk)

## Licenses
Copyright © 2024, NVIDIA Corporation. All rights reserved.

This work is made available under the NVIDIA Source Code License-NC. Click [here](https://github.com/nbasyl/DoRA/blob/main/LICENSE) to view a copy of this license.

## Citation
If you find DoRA useful, please cite it by using the following BibTeX entry.
```bibtex
@article{liu2024dora,
  title={{DoRA}: Weight-Decomposed Low-Rank Adaptation},
  author={Liu, Shih-Yang and Wang, Chien-Yi and Yin, Hongxu and Molchanov, Pavlo and Wang, Yu-Chiang Frank and Cheng, Kwang-Ting and Chen, Min-Hung},
  booktitle={arXiv:2402.09353},
  url={arxiv.org/abs/2402.09353},
  year={2024}
}
```
