# ROB535 Final Project: Monocular 3D Object detection

This repo is accommodated from [MonoCon](https://github.com/2gunsu/monocon-pytorch) for the NA565 final project. 

[You can find the report link here](https://github.com/Kgabel/Final-Project-ROB535-MONOCON/blob/c09d477149a8ffd56653db05a9f4e2a4ad64e8e4/Final_project_report.pdf)


Proposed Monocular 3-D object detection and modified monocular algorithm was implemented to calculate and localize 3D location of cars and their respective 3D bounding boxes in the input raw image data from camera. This project was a challenging problem as achieving a high accuracy required detecting vehicles in adversarial weather. To account for adverse weather, not only monocular’s configuration/paramters were modified but filters(nighttime and fog) were used to modify our train dataset to achieve AP score on testing sets.

The method utilizes a modified MonoCon monocular 3D detection algorithm with a 34-layer DLA deep neural network backbone. It has regression heads to estimate 3D box center offsets, depth, dimensions, observation angle, etc. It also uses auxiliary task branches. To enable operation in adverse weather, the training dataset is augmented by applying adjustable synthetic fog and raindrop filters to simulate challenging visibility conditions. Specifically, a fog layer is blended with the original image based on an intensity parameter. Raindrop masks with randomized RGB intensities are overlaid. The fog layer with droplets is then blended back onto images. The model is retrained on this augmented dataset using a weighted multi-task loss function comprising heatmap focal loss, uncertainty-aware depth loss, dimension-aware L1 loss, cross entropy classification loss, and box offset L1 loss.

![alt text](https://github.com/Kgabel/Final-Project-ROB535-MONOCON/blob/main/config/Screenshot%202023-12-31%2002154.png)

After setting up the envionment and converting the final project dataset into the KITTI format, the training code should work directly. 
## Preparations

### Setup
```bash
# [Step 1]: Create new conda environment and activate.
#           Set [ENV_NAME] freely to any name you want. (Please exclude the brackets.)
conda create --name [ENV_NAME] python=3.8
conda activate [ENV_NAME]

# [Step 2]: Clone this repository and change directory.
git clone https://github.com/2gunsu/monocon-pytorch
cd monocon-pytorch

# [Step 3]: See https://pytorch.org/get-started/locally/ and install pytorch for your environment.
#           We have tested on version 1.11.0.
#           It is recommended to install version 1.7.0 or higher.

# [Step 4]: Install some packages using 'requirements.txt' in the repository.
#           The version of numpy must be 1.22.4.
pip install -r requirements.txt

# [Step 5]
conda install cudatoolkit
```

### Environments

#### [Environment 1]
| OS                 | Python       | Pytorch      | CUDA         | GPU                   | NVIDIA Driver |
| :----------------: | :----------: | :----------: | :----------: | :-------------------: | :-----------: |
| Ubuntu 18.04.5 LTS | 3.8.13       | 1.11.0       | 11.4         | NVIDIA RTX A6000      | 470.129.06    |

#### [Environment 2]
| OS                 | Python       | Pytorch      | CUDA         | GPU                   | NVIDIA Driver |
| :----------------: | :----------: | :----------: | :----------: | :-------------------: | :-----------: |
| Ubuntu 20.04.6 LTS | 3.8.16       | 1.13.1       | 11.7         | NVIDIA RTX 4090       | 530.41.03     |

#### [Environment 3]
| OS                 | Python       | Pytorch      | CUDA         | GPU                   | NVIDIA Driver |
| :----------------: | :----------: | :----------: | :----------: | :-------------------: | :-----------: |
| Ubuntu 20.04.6 LTS | 3.8.16       | 2.0.1        | 11.8         | NVIDIA RTX 4090       | 530.41.03     |





The structure of the data files should be as below.
```bash
[ROOT]
│
├── training
│   ├── calib
│   │   ├── 000000.txt
│   │   ├── 000001.txt
│   │   └── ...
│   ├── image_2
│   │   ├── 000000.png
│   │   ├── 000001.png
│   │   └── ...
│   └── label_2
│       ├── 000000.txt
│       ├── 000001.txt
│       └── ...
│
└── testing
    ├── calib
    └── image_2
```
![alt text](https://github.com/Kgabel/Final-Project-ROB535-MONOCON/blob/main/config/Screenshot%202023-12-31%20015717.png)


## Citation
```latex
@InProceedings{liu2022monocon,
    title={Learning Auxiliary Monocular Contexts Helps Monocular 3D Object Detection},
    author={Xianpeng Liu, Nan Xue, Tianfu Wu},
    booktitle = {36th AAAI Conference on Artifical Intelligence (AAAI)},
    month = {Feburary},
    year = {2022}
}
```


## References
The following repositories were referred.  
- [MonoCon](https://github.com/Xianpeng919/MonoCon)
- [mmdetection3d](https://github.com/open-mmlab/mmdetection3d)
- [nuScenes-devkit](https://github.com/nutonomy/nuscenes-devkit)
