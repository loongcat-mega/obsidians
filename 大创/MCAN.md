## Abstract


Visual Question Answering (VQA) requires a finegrained and simultaneous understanding of both the visual content of images and the textual content of questions.

we propose a deep Modular Co-Attention Network (MCAN) that consists of Modular Co-Attention (MCA) layers cascaded in depth


Each MCA layer models the self-attention of questions and images, as well as the guided-attention of images jointly using a modular composition of two basic attention units.



Modular 模块化的

extensive  广泛的

conduct  进行,执行

ablation 消融

quantitatively and qualitatively 定量和定性

## Introduction

Multimodal learning 多模态学习

semantic 语义

reasoning 推理 推论

unimodal 单峰的

insufficient 不足的

facilitates 促进

form 出现，建立，表格

thereby  从而

potentially  潜在地 可能地

corresponding 相应的 相关的

counterparts  同行

bottleneck 瓶颈

verifies 证实 验证

synergy 协同作用


## Related  Work

VQA has been of increasing interest over the last few years.

fusion 融合 结合

formulated 规划 制定的

mechanism 机制

alternately 交替地

refine 精炼 改进

separate 单独地 分开的 分离


## Modular Co-Attention Layer


simplicity  质朴 简单 容易
 
attended 参加 注意
 
weighted summation 加权求和

with respect to 关于 就...而言

residual  残差

Interpretation 解释


## Modular Co-Attention Networks


intermediate  居中的，中间的

trimmed 休整过的，修建，减少

fill

maximum

minimum

The input image is represented as a set of regional visual features in a bottom-up manner [1]. These features are the intermediate features extracted from a Faster R-CNN model (with ResNet-101 as its backbone) [26] pre-trained on the Visual Genome dataset [18]. We set a confidence threshold to the probabilities of detected objects and obtain a dynamic number of objects m ∈ [10, 100]. For the i-th object, it is represented as a feature xi ∈ Rdx by mean-pooling the convolutional feature from its detected region. Finally, the image is represented as a feature matrix X ∈ Rm×dx . 


The input question is first tokenized into words, and trimmed to a maximum of 14 words similar to [28, 14]. Each word in the question is further transformed into a vector using the 300-D GloVe word embeddings [25] pretrained on a large-scale corpus. This results in a sequence of words of size n×300, where n ∈ [1, 14] is the number of words in the question. The word embeddings are then passed through a one-layer LSTM network [13] with dy hidden units. In contrast to [28] which only uses the final state (i.e., the output feature for the last word) as the


regions

analogy


zotero








