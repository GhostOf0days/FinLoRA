# FinLoRA: Finetuning Qauntized Financial Large Language Models using Low-Rank Adaptation

## Introduction

### Motivation

The closed-source BloombergGPT was announced in April 2023, then the financial sector started to value FinLLMs. However,
its train-from-scratch approach requires millions of GPU hours, which is too expensive. Instead, we adopt the LoRA
fine-tuning approach to leverage open-source models like Llama. The trainable parameters are reduced to only 0.01% of
the full parameters. The trainable parameters are reduced to as low as only 0.01% of the full parameters.

### Current XBRL Tasks

| Name                  | Category | Train samples | Link |
|:----------------------|:---------|:--------------|:-----|
| XBRL Tags Extraction  | XBRL QA  | > 500         | -    |
| XBRL Value Extraction | XBRL qA  | > 2K          | -    |


### Additional XBRL Tasks

[//]: # (refernec to paper)

Current XBRL tasks are limit in variety and the datasets are new and might be perceived as unreliable, therefore more
established XBRL tasks are required.

[//]: # (TODO: this table in paper appendix)

| Name                             | Category         | Train samples | Link                                                                                                                          |
|:---------------------------------|:-----------------|:--------------|:------------------------------------------------------------------------------------------------------------------------------|
| FiNER                            | XBRL Tagging     | 900K          | [HF](https://huggingface.co/datasets/nlpaueb/finer-139?row=16)                                                                |
| FNXL                             | XBRL Tagging     | 1K            | [GitHub](https://github.com/soummyaah/FNXL)                                                                                   |
| XBRL Term                        | XBRL Terminology | -             | [GitHub](https://github.com/KirkHan0920/XBRL-Agent/blob/main/Datasets/XBRL%20Terminology.xlsx)                                |
| Financial Math                   | Math             | -             | [GitHub](https://github.com/KirkHan0920/XBRL-Agent/blob/main/Datasets/formulas_with_explanations_with_questions_with_gt.xlsx) |
| XBRL Formula Formatted with Tags | XBRL QA          | > 2K          | -                                                                                                                             |
| XBRL Formula Calculations        | XBRL QA          | > 2K          | -                                                                                                                             |

### Cross-task Generalization (LoRA MoE)

Currently we finetune one LoRA adaptor for every task. Although single-task finetuning have higher performance, it might
not be practical in application.

Mixture of LoRA Experts (LoRA MoE): each LoRA module acts as an expert, a router network assigns the LoRA weights. One
implementation is [X-LoRA](https://arxiv.org/pdf/2402.07148)[4]. X-LoRA is built on top of huggingface PEFT, therefore
implementation should be relatively straightforward.

### Improve Performance and Scalability for Inference

SLoRA[5] is designed for the serving of many LoRA adapters efficiently. It stores all adapters in the memory and
fetches the adapters needed to GPU memory. It might be possible to use some of the ideas of SLoRA with LoRA MoE for a
more
efficient implementation of LoRA MoE.

Difficulty: Current SLoRA implementation does not work with HuggingFace, and does not support newer model like Llama 3.

### Federated Learning with Enhanced Privacy

Multiple institutions might want to collaborate for finetuning using combined data using Federated Learning.
Differentially Private Low-Rank Adaptation (DP-LoRA) [5] offers an approach using federated learning and by adding noise
in weight updates to avoid inferring sensitive information from model outputs. Adding zero-knowledge learning on top of
DP-LoRA allows additional privacy.

[//]: # (Different user base, our model serve community, open-source well, we use finetuning)

[//]: # (assume large amount of user: )

[//]: # (e)

[//]: # (percentage)

[//]: # (compare results with icdcs)

## References

[1] Xiao-Yang Liu, Jie Zhang, Guoxuan Wang, Weiqing Tong, Anwar Walid. FinGPT-HPC: Efficient Pretraining and Finetuning
Large Language Models for Financial Applications with High-Performance Computing. IEEE ICDCS 2024.

[2] Mao, Y., Ge, Y., Fan, Y., Xu, W., Mi, Y., Hu, Z. and Gao, Y., 2024. A Survey on LoRA of Large Language Models. arXiv
preprint arXiv:2407.11046.

[3] Vlad Fomenko, Han Yu, Jongho Lee, Stanley Hsieh, Weizhu Chen. A Note on LoRA, 2024. https://arxiv.org/abs/2404.05086

[4] E.L. Buehler, M.J. Buehler. X-LoRA: Mixture of Low-Rank Adapter Experts, a Flexible Framework for Large Language
Models with Applications in Protein Mechanics and Design}, https://arxiv.org/abs/2402.07148

[5] Sheng, Ying and Cao, Shiyi and Li. Dacheng and Hooper, et al. S-LoRA: Serving Thousands of Concurrent LoRA
Adapters, https://arxiv.org/pdf/2311.03285

[6] Xiao-Yang Liu, Rongyi Zhu, Daochen Zha, Jiechao Gao, Shan Zhong, Matt White, Meikang Qiu,
Differentially Private Low-Rank Adaptation of Large Language Model Using Federated
Learning, https://arxiv.org/abs/2312.17493
