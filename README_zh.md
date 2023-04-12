# 用于小型和高分辨率图像集的快速稳定GAN，pytorch版本
论文“Towards Faster and Stabilized GAN Training for High-fidelity Few-shot Image Synthesis”的官方pytorch实现，该论文可以这里找到 [here](https://arxiv.org/abs/2101.04775).

## 0. Data
论文中使用的数据集可以在找到 [link](https://drive.google.com/file/d/1aAJCZbXNHyraJ6Mi13dSbe7pTyfPXha0/view?usp=sharing). 

在20多个数据集上测试后，每个数据集的图像少于100张，该GAN在其中80%上收敛。对于这个GAN可以聚合的数据集，我仍然无法总结出明显的“良好财产”模式，请随意尝试使用您自己的数据集。

## 1. Description
代码的结构如下:
* models.py: 所有模型的结构定义.

* operation.py: 训练过程中的辅助功能和数据加载方法.

* train.py: 代码的主条目，执行该文件来训练模型，中间结果和检查点将自动定期保存到文件夹“train_results”中.

* eval.py: 将经过训练的生成器中的图像生成到文件夹中，该文件夹可用于计算FID分数.

* benchmarking: 我们用来计算FID的函数位于这里，它会自动下载pytorch官方的初始模型. 

* lpips: 该文件夹包含计算LPIPS分数的代码，初始模型也会自动从官方位置下载.

* scripts: 该文件夹包含许多脚本，您可以使用这些脚本来处理经过训练的模型。包括: 
    1. style_mix.py: 论文中介绍的风格混合;
    2. generate_video.py: 从生成的图像的插值生成连续视频;
    3. find_nearest_neighbor.py: 给定生成的图像，从训练集中找到最接近的真实图像;
    4. train_backtracking_one.py: 给定真实图像，从经过训练的生成器中找到该图像的潜在向量.

## 2. How to run
将所有训练图像放在一个文件夹中，运行:
```
python train.py --path /path/to/RGB-image-folder
```
您还可以查看所有训练选项:
```
python train.py --help
```
代码将自动创建一个新文件夹（您必须使用--name选项指定文件夹的名称），以存储经过训练的检查点和中间合成结果.

完成训练后，您可以通过以下方式生成100张图像（或任意数量）:
```
cd ./train_results/name_of_your_training/
python eval.py --n_sample 100 
```

## 3. Pre-trained models
共享预训练模型和每个模型的相应代码 [here](https://drive.google.com/drive/folders/1nCpr84nKkrs9-aVMET5h8gqFbUYJRPLR?usp=sharing).

您还可以使用FastGAN生成带有预打包Docker映像的映像，该映像托管在Replicate注册表中: https://beta.replicate.ai/odegeasslbc/FastGAN

## 4. Important notes
1. 提供的代码仅供研究使用.
2. 在不同的数据集上需要不同的模型和训练配置。您可能需要调整超参数，以便在自己的数据集上获得最佳结果. 

    2.1. 超参数包括：增强选项、模型深度（层数）、模型宽度（每层的通道数）。要更改这些，您必须直接更改models.py和train.py中的代码. 
    
    2.2. 请检查共享预训练模型中的代码，了解如何在不同的数据集上对每个模型进行不同的配置。特别是，比较ffhq和art数据集的models.py，你会知道在不同的数据集上会发生什么变化.

## 5. Other notes
1. 提供的脚本组织不完善，欢迎投稿清理.
2. 本文的第三方实现可以在 [here](https://github.com/lucidrains/lightweight-gan)找到，其中包括一些其他技术。如果发现其中一个不起作用，我建议您尝试两种实现. 
