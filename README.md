| 第三届计图挑战赛

# Jittor 大规模无监督语义分割-基于PASS的无监督图像分割

![主要结果](https://cloud.tsinghua.edu.cn/f/3c1473f9747644438676/)
同时存于同级文件夹的PASS_result.zip中

在A榜test集上经模型测试结果如链接内zip文件内所示

## 简介
本项目包含了第三届计图挑战赛计图 大规模无监督语义分割赛题的代码实现。是在无监督的情形下，由随机初始化的模型使用代理任务的自我监督进行训练，学习形状、类别表示，之后进行聚类处理，
获取伪类别并分配，再由此对预训练的模型进行微调，优化分割，完成可生成伪标签分割图的模型。

## 安装 
硬件配置:本项目在单张A6000上运行，训练时间约为一天半


#### 运行环境
- ubuntu 20.04.6 LTS
- python >= 3.8
- jittor  1.3.7.16

#### 安装依赖
执行以下命令安装 python 依赖
```
pip install -r requirements.txt
```

#### 预训练模型
该项目无预训练模型，为随机初始模型，做自监督训练

##.sh文件适配更改
需依次将PASS/luss50_pass文件夹中从1-8的.sh文件，文件头中DATA和IMAGENETS改为
自身ImageNetS50数据集所在文件夹路径

## 训练

依次运行以下命令：
```
bash scripts/luss50_pass/1_pretrain.sh
bash scripts/luss50_pass/2_pixel_attention.sh
bash scripts/luss50_pass/3_cluster.sh
bash scripts/luss50_pass/4_eval.sh
bash scripts/luss50_pass/5_inference_pixel_atte.sh
bash scripts/luss50_pass/6_finetune.sh
bash scripts/luss50_pass/7_eval.sh
```

## 推理
｜ 介绍模型推理、测试、或者评估的方法

生成测试集上的结果可以运行以下命令：

```
bash scripts/luss50_pass/8_test.sh
```

## 注意事项
偶发现象:因数据加载的库实现问题,由于多线程加载数据关系,偶发在执行脚本时个别图片尚未经过模型处理生成下一步所需图片即被覆盖,一般重新执行报错步骤上一步的脚本即可,若
处理后仍无法解决,则将所有涉及num_workers的部分手动设置为1或0(强制单线程工作),但这样会降低训练效率.
