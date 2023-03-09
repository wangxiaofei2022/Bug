# 运行过程中遇到的Bug

1.安装imgaug库时，直接用pip install imgaug报错 Failed to build Shapely

![image](https://github.com/wangxiaofei2022/Bug/blob/main/Failed_to_build_Shapely.png)

解决办法：

https://zhenkai.blog.csdn.net/article/details/89602237?spm=1001.2101.3001.6650.6&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EESLANDING%7Edefault-6-89602237-blog-85080630.pc_relevant_landingrelevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EESLANDING%7Edefault-6-89602237-blog-85080630.pc_relevant_landingrelevant&utm_relevant_index=7

（1）先安装“shapely”依赖（用conda命令，直接用pip可能安装不上Shapely）

        conda install Shapely

（2）再运行以下安装imgaug的命令

        pip install imgaug

2.运行CUDA_VISIBLE_DEVICES=7 python tools/test.py configs/swin/mask_rcnn_swin-t-p4-w7_fpn_1x_coco.py work_dirs/swin/epoch_24.pth  --eval bbox时遇到的问题：

![image](https://github.com/wangxiaofei2022/Bug/blob/main/IndexError_list_index_out_of_range.png)

解决办法：

https://github.com/open-mmlab/mmdetection/issues/4243

将configs/_base_/datasets/目录下的**coco_detection.py**和**coco_instance.py**添加classes = ('car', 'truck')  classes=classes,

        ...
        dataset_type = 'CocoDataset'
        classes = ('car', 'truck')
        ...
        data = dict(
            samples_per_gpu=2,
            workers_per_gpu=2,
            train=dict(
                type=dataset_type,
                classes=classes,
                ann_file='path/to/your/train/data',
                ...),
            val=dict(
                type=dataset_type,
                classes=classes,
                ann_file='path/to/your/val/data',
                ...),
            test=dict(
                type=dataset_type,
                classes=classes,
                ann_file='path/to/your/test/data',
                ...))
        ...

