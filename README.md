# 3D-to-2D Distillation for Indoor Scene Parsing

### Introduction

This repository is the implementation for 3D-to-2D Distillation for Indoor Scene Parsing in CVPR 2021 (Oral). The code is based on [PSPNet](https://github.com/hszhao/semseg ).

### Usage

1. Requirement:

   - Hardware: 4 GPUs (better with >=11G GPU memory)
   - Software: PyTorch>=1.1.0, Python3, [tensorboardX](https://github.com/lanpa/tensorboardX), 

2. Clone the repository:

   ```shell
   git clone https://github.com/liuzhengzhe/3D-to-2D-Distallation-for-Indoor-Scene-Parsing
   ```

4. Train:

   - Download ScanNet-v2 2D data scannet_frames_25k (http://www.scan-net.org/) and 3D features extracted from MinkowskiNet (https://github.com/chrischoy/SpatioTemporalSegmentation), including folders named  "eachfeature" and "featidx2". Then put to them in the data folder:

   - Ignore 20 unused categories in ScanNet and generate new ground truth
     ```shell
	 python data_process1.py
	 python data_process2.py
	 ```

   - Download ImageNet pre-trained [models]((https://drive.google.com/open?id=15wx9vOM0euyizq-M1uINgN0_wjVRf9J3)) provided by https://github.com/hszhao/semseg and put them under folder `initmodel` for weight initialization. 
   
   - Specify the gpu used in config then do training:

     ```shell
     sh tool/train.sh scannet pspnet50
     ```

5. Test:

   - Download trained segmentation models and put them under folder specified in config or modify the specified paths.

   - For full testing (get listed performance):

     ```shell
     sh tool/test.sh scannet pspnet50
     ```
	 
6. Generate 3D features from other 3D semantic segmentation models


   - Run 3D semantic segmentation model and save the features in the "data/feat" folder. Each file contains a feature array for one point cloud with shape N*d, where N is the number of points and d is the dimension of the 3D feature. The order of features should follow the order of points of the ScanNet .ply file. 

     ```shell
     cd data/
     python proj3dto2d_1.py
     python proj3dto2d_2.py
     ```
	 
	 Then you can train the model with other custom 3D feature. 


### Performance

Description: **mIoU/mAcc/aAcc** stands for mean IoU, mean accuracy of each class and all pixel accuracy respectively. 

 **ScanNet-v2**:

   - Setting: train on **train** set and test on **val** set. 

   |  Backbone | mIoU/mAcc/pAcc(ms)   
   | :-------: | :-------------------: 
   | PSPNet50  | 0.5822/0.7083/0.8170. 
