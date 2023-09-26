### README

#### 环境配置

参照https://github.com/zldscr0/WSI-FT中p39环境的配置，其他依赖项通过执行下面的指令安装。

```bash
 pip install -r requirement.txt
```

#### 数据集

##### WSI Segmentation and Patching
```bash
CUDA_VISIBLE_DEVICES=0,1 python extract_features_fp.py --data_h5_dir DIR_TO_COORDS --data_slide_dir DATA_DIRECTORY --csv_path CSV_FILE_NAME --feat_dir FEATURES_DIRECTORY --batch_size 512 --slide_ext .svs
```

##### Feature Extraction

```bash
CUDA_VISIBLE_DEVICES=0,1 python extract_features_fp.py --data_h5_dir DIR_TO_COORDS --data_slide_dir DATA_DIRECTORY --csv_path CSV_FILE_NAME --feat_dir FEATURES_DIRECTORY --batch_size 512 --slide_ext .svs
```

这两个处理数据集的部分也可以直接使用之前的`WSI-finetuning`复现中写的两个脚本https://github.com/zldscr0/WSI-FT

> 处理好的图像在本地服务器上存储位置为`/data/Colon/feat_dir`

#### 参数修改

1.修改`TransMIL.yaml`中的

`dataset_name`：数据集名称，需同`TransMIL/datasets`下的数据集构造相关的`.py`文件一一对应。

`data_dir`：CLAM框架执行完提取特征（上述的Feature Extraction）后的`.pt`文件

`label_dir`：存带标签信息（已经划分好训练集、测试集和验证集）的`.csv`文件的目录，`.csv`的文件示例参照代码库中的`dataset_csv/final.csv`

![image-20230926084626349](C:\Users\hanabi\AppData\Roaming\Typora\typora-user-images\image-20230926084626349.png)

2.修改`TransMIL.yaml`中的其他相关超参数开始运行。

#### train

```bash
python train.py --stage='train' --config='TransMIL.yaml'  --gpus=0 --fold=0
```

##### 训练过程截图

![image-20230926091511801](C:\Users\hanabi\AppData\Roaming\Typora\typora-user-images\image-20230926091511801.png)

##### 最优模型

自动保存在`logs/`下，训练过程中验证集的结果存在`metrics.csv`中

![image-20230926091621266](C:\Users\hanabi\AppData\Roaming\Typora\typora-user-images\image-20230926091621266.png)

#### test

对最优模型进行测试：

```bash
python train.py --stage='test' --config='TransMIL.yaml'  --gpus=0 --fold=0
```

##### 测试结果

> logs/fold0/result.csv

![image-20230925090413156](C:\Users\hanabi\AppData\Roaming\Typora\typora-user-images\image-20230925090413156.png)

#### Loss图

![image-20230926092242051](C:\Users\hanabi\AppData\Roaming\Typora\typora-user-images\image-20230926092242051.png)
