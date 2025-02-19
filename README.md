<!--
 * @Author: fuchy@stu.pku.edu.cn
 * @Date: 2021-09-18 18:33:55
 * @LastEditTime: 2021-12-14 20:09:55
 * @LastEditors: FCY
 * @Description: README
 * @FilePath: /compression/README.md
-->
# OctAttention: Octree-Based Large-Scale Contexts Model for Point Cloud Compression

## Branches

There are two branches named `obj` and `lidar` that implement Object and LiDAR point cloud coding respectively. They share the same network. [Paper](http://arxiv.org/abs/2202.06028).

## Requirements
- python 3.7
- PyTorch 1.9.0+cu102
- `file/environment.sh` to help you build this environment

## Download and Prepare Training and Testing Data
- ### Download data
    ### For LiDAR compression
    
    [SemanticKITTI](http://www.semantic-kitti.org/dataset.html) (80G)  
    23201/20351 frames in 00-10/11-21 folders for training/testing. 

    ### For Object compression

    [MPEG 8iVFBv2](http://plenodb.jpeg.org/pc/8ilabs)  (5.5GB)  
    300/300 frames in soldier10 and longdress10 for training  
    300/300 frames in loot10 and redandblack10 for testing
    
    [MPEG 8iVFB](https://disk.pku.edu.cn/#/link/E08699D04D09244C82E50D0C44093F34%20%E6%9C%89%E6%95%88%E6%9C%9F%E9%99%90%EF%BC%9A2025-08-31%2023:59) (100M)  
    1/1/1/1 frame in Boxer9/10 and Thaidancer9/10 (quantized from 12bit data) for testing

    [JPEG MVUB](http://plenodb.jpeg.org/pc/microsoft) (8GB)  
    318/216/207 frames in andrew10, david10 and sarah10 for training  
    245/245/216/216 frames in Phil9/10 and Ricardo9/10 for testing  
    (Note: We rotated the MVUB data to make it consistent with MPEG 8i. Please set `rotation=True` in the `dataPrepare` function when processing MVUB data in training and testing.)

- ### Prepare data
Please set `oriDir` in `dataPrepare.py` before. 
```
python dataPrepare.py
```
To prepare train and test data. It will generate `*.mat` data in the directory `Data`.  
    
## Train
```
python octAttention.py 
```
You should set the Network parameters `expName,DataRoot`etc. in `networkTool.py`.
This will output checkpoint in `expName` folder, e.g. `Exp/Kitti`. (Note: You should run `DataFolder.calcdataLenPerFile()` in `dataset.py` for a new dataset, and you can comment it after you get the parameter `dataLenPerFile`)

## Encode and Decode
You may need to run the following command to provide `pc_error` and `tmc13v14` execute permission.
```
chmod +x file/pc_error file/tmc13v14 
``` 
- ### Encode
```
python encoder.py  
```
This will output binary codes saved in `.bin` format in `Exp(expName)/data`, and will generate `*.mat` data in the directory `Data/testPly`.

- ### Decode
```
python decoder.py 
```
This will load `*.mat` data for check and calculate PSNR by `pc_error`.

## Test TMC
We provide the test code for [TMC13](https://github.com/MPEGGroup/mpeg-pcc-tmc13) v14 (G-PCC) for Object and LiDAR point cloud compression.
```
python testTMC.py
```

## Citation
If this work is useful for your research, please consider citing :

    @inproceedings{OctAttention,
    title={OctAttention: Octree-Based Large-Scale Contexts Model for Point Cloud Compression},
    author={Fu, Chunyang and Li, Ge and Song, Rui and Gao, Wei and Liu, Shan},
    booktitle={Proceedings of the AAAI Conference on Artificial Intelligence (AAAI)},
    year={2022}
    }
