[ViT（Vision Transformer）解析](https://zhuanlan.zhihu.com/p/445122996#Popover19-toggle:~:text=ViT%EF%BC%88Vision%20Transformer%EF%BC%89%E8%A7%A3%E6%9E%90)

ViT是2020年Google团队提出的将Transformer应用在图像分类的模型

ViT将输入图片分为多个patch（16x16），再将每个patch投影为固定长度的向量送入Transformer，后续encoder的操作和原始Transformer中完全相同。但是因为对图片分类，因此在输入序列中加入一个特殊的token，该token对应的输出即为最后的类别预测
![[Pasted image 20230419222342.png]]
