# 运行过程中遇到的Bug

1.



2.运行CUDA_VISIBLE_DEVICES=7 python tools/test.py configs/swin/mask_rcnn_swin-t-p4-w7_fpn_1x_coco.py work_dirs/swin/epoch_24.pth  --eval bbox时遇到的问题：

![image](https://github.com/wangxiaofei2022/Mmdetection/blob/main/IndexError_list_index_out_of_range.png)

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

