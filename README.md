<h1 align="center">
    <p>DoRA: Weight-Decomposed Low-Rank Adaptation</p>
</h1>

<h1 align="center"> 
    <img src="./dora.png" width="600">
</h1>

The official implementation of [DoRA](https://arxiv.org/abs/2402.09353) can be found [here](https://github.com/huggingface/peft/blob/main/src/peft/tuners/lora/layer.py) and is now fully supported by HuggingFace PEFT.

> [!IMPORTANT]
> The code and checkpoints for reproducing the results of the four experiments reported in the paper will be released soon. Please be aware that releasing the code and checkpoints to the public necessitates an internal review process within the company, which can be time-consuming. We appreciate your understanding on this matter.

## DoRA: Weight-Decomposed Low-Rank Adaptation

Shih-Yang Liu, Chien-Yi Wang, Hongxu Yin, Pavlo Molchanov, Yu-Chiang Frank Wang, Kwang-Ting Cheng, Min-Hung Chen

Paper: https://arxiv.org/abs/2402.09353

Project page: https://nbasyl.github.io/DoRA-project-page/

DoRA decomposes the pre-trained weight into two components, magnitude and direction, for fine-tuning, specifically employing LoRA for directional updates to efficiently minimize the number of trainable parameters. By employing DoRA, we enhance both the learning capacity and training stability of LoRA while avoiding any additional inference overhead. DoRA consistently outperforms LoRA on fine-tuning LLaMA, LLaVA, and VL-BART on various downstream tasks, such as commonsense reasoning, visual instruction tuning, and image/video-text understanding.

## News
2024/3/13: QDoRA: DoRA with BnB quantization and Conv2D DoRA is now supported by the Huggingface PEFT package!

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

### HuggingFace Diffusers
You can also toy with DoRA on finetuning diffusion models. See [huggingface/diffusers](https://github.com/huggingface/diffusers/tree/main/examples/advanced_diffusion_training#dora-training). Another good tutorial would be this [Colab notebook](https://colab.research.google.com/drive/134mt7bCMKtCYyYzETfEGKXT1J6J50ydT?usp=sharing#scrollTo=23d6bb49-3469-4e23-baf5-25b2344b599d) from [Linoy Tsaban](https://twitter.com/linoy_tsaban).

In general, DoRA finetuning on diffusion model is still _experimental_ and is likely to require different hyperparameter values to perform best compared to LoRA.

> [!NOTE]
> Specifically, people have noticed 2 differences to take into account your training: 
> 1. **LoRA seem to converge faster than DoRA** (so a set of parameters that may lead to overfitting when training a LoRA may be working well for a DoRA)
> 2. **DoRA quality superior to LoRA especially in lower ranks** the difference in quality of DoRA of rank 8 and LoRA of rank 8 appears to be more significant than when training ranks of 32 or 64 for example.  

#### Some DoRA v.s LoRA diffusion finetuning results
- Example From [Linoy Tsaban](https://twitter.com/linoy_tsaban) (Images generated by DoRA are on the left and LoRA on the right):
<h1 align="center"> 
    <img src="./dora_lora_yoda_emoji.jpg" width="500">
</h1>

- Example From [merve](https://twitter.com/mervenoyann):
<h1 align="center"> 
    <img src="./dora_lora_lego.jpeg" width="500">
</h1>


### DoRA hyperparameters settings
> [!NOTE]
> 💡 While fine-tuning with DoRA, by utilizing the configuration of LoRA can already achieve better results most of the time, achieving optimal performance compared to LoRA still requires adjustments to the hyperparameters. 
>
> We suggest starting with a slightly lower learning rate than that of LoRA, and users may also experiment with varying lora dropout ratios.
>
> User may also start with half of the rank of the LoRA configuration which oftentime can already results in comparable or even superior accuracy compared to that of LoRA.

## Reproducing the results in the paper

The code will be released under https://github.com/NVlabs soon and will contain four directories:

`./commonsense_reasoning` contains the code to finetune LLaMA-7B/13B using DoRA on the commonsense reasoning tasks. This directory is modified based on [LLM-Adapter](https://github.com/AGI-Edgerunners/LLM-Adapters).

`./instruction_tuning` contains the code to finetune LLaMA-7B and LLaMA2-7B using DoRA and DVoRA (DoRA+VeRA) with the cleaned Alpaca instruction tuning dataset. This directory is modified based on [VeRA](https://openreview.net/attachment?id=NjNfLdxr3A&name=supplementary_material).

`./image_video_text_understanding` contains the code to finetune VL-BART using DoRA for the image/video-text understanding tasks. This directory is modified based on [VL-Adapter](https://github.com/ylsung/VL_adapter).

`./visual_instruction_tuning` contains the code to finetune LLaVA-1.5-7B on the visual instruction tuning tasks with DoRA. This directory is modified based on [LLaVA](https://github.com/haotian-liu/LLaVA).

### For those who really wish to try reproducing the results of the paper before our code is released:
1. You can start with reproducing the results in table 1, try replacing the PEFT package in the [LLM-Adapter](https://github.com/AGI-Edgerunners/LLM-Adapters) code base with the latest version of PEFT which supports DoRA finetuning. 
2. Please install the python packages with [the provided requirements.txt](https://github.com/nbasyl/DoRA/blob/main/commonsense_reasoning/requirements.txt), since there are some issue with the latest version of `accelerate` and `transformers` package.
3. Use the hyperparameter specified in the paper for finetuning LLaMA-7B/13B.

## Contact
Shih-Yang Liu: [shihyangl@nvidia.com](shihyangl@nvidia.com) or [sliuau@connect.ust.hk](sliuau@connect.ust.hk)

## Licenses
Copyright © 2024, NVIDIA Corporation. All rights reserved.

This work is made available under the NVIDIA Source Code License-NC. Click [here](https://github.com/nbasyl/DoRA/blob/main/LICENSE) to view a copy of this license.

## Citation
If you find DoRA useful, please cite it by using the following BibTeX entry.
```bibtex
@article{liu2024dora,
  title={DoRA: Weight-Decomposed Low-Rank Adaptation},
  author={Liu, Shih-Yang and Wang, Chien-Yi and Yin, Hongxu and Molchanov, Pavlo and Wang, Yu-Chiang Frank and Cheng, Kwang-Ting and Chen, Min-Hung},
  journal={arXiv preprint arXiv:2402.09353},
  year={2024}
}
```
