# vqa

去了解更多vqa以及视觉/语言模型的特点

# Unified Vision-Language Pre-Training for Image Captioning and VQA



本文介绍了一种统一的视觉语言预训练(VLP)模型。 该模型的统一性表现在： (1)它可以针
对视觉语言生成(例如图像描述)或理解(例如视觉问答)任务进行微调； (2)它使用了一个共享的多
层Transformer网络进行编码和解码,这与许多现有方法不同,后者使用分离的模型实现编码器和解码
器。统一的VLP模型使用无监督学习目标对大量图像文本对进行预训练,这两个任务是双向和序列到
序列(seq2seq)掩码视觉语言预测。这两个任务的不同之处仅在于预测条件所在的上下文。这是通过
利用特定的自注意力掩码来控制共享Transformer网络实现的。据我们所知,VLP是第一个在图像描述
和视觉问答等截然不同的视觉语言生成和理解任务上均取得最先进结果的模型,在三个具有挑战性的
基准数据集上(COCO Captions、Flickr30k Captions和VQA 2.0)都表现出色。代码和预训练模型可
在https://github.com/LuoweiZhou/VLP找到。



好像是UniLM的魔改？？

