## 基于 PyTorch 的 SSD 目标检测算法实现
---

### 目录
1. [训练步骤与使用方法](#训练步骤与使用方法)
2. [检测方法的使用](#检测方法的使用)


### 训练步骤与使用方法
a. 在 [classes.txt](./classes.txt) 文件填上类别名称，一行一个类别。
b. [label.txt](./label.txt) 里是训练数据的信息。一行文本就是一个图片的数据信息。每一行文本诸如以下形式：
```
path/to/image.jpg x1,y1,x2,y2,classes_name;x1,y1,x2,y2,classes_name;...
```
比如
```
/home/data/train/708.jpg 336,508,584,758,dog;13,100,175,263,bottle;336,508,584,758,dog
```
表示图片 /home/data/train/708.jpg 包含3个物体，分别为 dog，bottle，dog；每一个类别名前面的4个数字就是该物体的左上角与右下角坐标。
c. 设置配置文件 [config.py](./config.py)
    "cuda": 表示是否使用 cuda（实际上如果 cuda 设为 True，如果在无 GPU 机器上还是会使用 cpu且不会报错）。
    "image_size": 原始图像大小。
    "min_sizes", "max_sizes": 预设框边长设置。
    "ratio": 预设框宽高比设置。
    "variance": 与 decode 和 encode 有关。

    实际根据检测物体的宽高比和大小修改"min_sizes"， "max_sizes"， "ratio"就可以了。
4. 训练。
运行 [train.py](./train.py)
```
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python -m torch.distributed.launch --nproc_per_node=8 train.py
```


### 检测方法的使用
运行 [detect.py](./detect.py)。
```
python detect.py
```
