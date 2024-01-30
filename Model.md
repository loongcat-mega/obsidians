
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240114151138.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240114152554.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240114152657.png)


## BLOOM

BLOOM BigScience Large Open-science Open-access Mul-tilingual Language Model
是一个有HuggingFace、GENCI和IDRIS发起的开放式协作组织。
BLOOM本身是Transformer解码器模型，在一个称之为ROOTS的语料库上训练出来的176B参数规模的自回归语言模型，也是第一个开源开放超过100B的语言模型，ROOTS的语料库包含46种自然语言和13种编程语言

## LLaMA
Large Language Model Meta AI
由 Meta AI 发布的一个开放且高效的大型基础语言模型，共有 `7B`、`13B`、`33B`、`65B`（650 亿）四种版本。其数据集来源都是公开数据集，无任何定制数据集，保证了其工作与开源兼容和可复现，整个训练数据集在 token 化之后大约包含 1.4T 的 token

Chinese-LLaMA-Alpaca。该项目在原版 LLaMA 的基础上扩充了中文词表并使用了中文数据进行二次预训练，进一步提升了中文基础语义理解能力。同时，在中文LLaMA 的基础上，本项目使用了中文指令数据进行指令精调，显著提升了模型对指令的理解和执行能力

Vicuna 是一款从 LLaMA 模型中对用户分享的对话进行了精细调优的聊天助手，根据的评估，这款聊天助手在 LLaMA 子孙模型中表现最佳，能达到 ChatGPT 90% 的效果

## BaiChuan

Baichuan-7B 是由百川智能开发的一个开源可商用的大规模预训练语言模型。基于 Transformer 结构，在大约 1.2 万亿 tokens 上训练的 70 亿参数模型，支持中英双语，上下文窗口长度为 4096。在标准的中文和英文 benchmark（C-Eval/MMLU）上均取得同尺寸最好的效果


## ziya

Ziya是一个基于LLaMa的130亿参数的中英双语预训练语言模型，它由**IDEA研究院认知计算与自然语言研究中心（CCNL）推出，是开源通用大模型系列的一员。Ziya具备翻译，编程，文本分类，信息抽取，摘要，文案生成，常识问答和数学计算等能力，可以处理多种自然语言任务

Ziya是一个**自回归**的模型，也就是说它只能从左到右生成文本，而不能同时使用上下文信息



```bash
python gradio_demo.py --model_type llama --base_model shibing624/ziya-llama-13b-medical-merged
```