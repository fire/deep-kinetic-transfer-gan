# Deep-motion-editing

This library provides fundamental and advanced functions to work with 3D character animation in deep learning with Pytorch. The code contains end-to-end modules, from reading and editing animation files to visualizing and rendering (using Blender) them.

## Prerequisites

- Linux or macOS
- Python 3
- CPU or NVIDIA GPU + CUDA CuDNN

## Quick Start

We provide pretrained models together with demo examples using animation files specified in bvh format.

<!--```bash
python test.py -model_path MODEL_PATH -input_A PATH_A -input_B PATH_B -edit_type TYPE
```-->

### Motion Retargeting

The retargeted demo results, that consists both intra-structual retargeting and cross-structural retargeting, will be saved in `retargeting/pretrained/results`.

### Motion Retargeting

#### Dataset

We use Mixamo dataset to train our model. You can download our preprocessed data from [Google Drive](https://drive.google.com/file/d/1BYH2t5XMGWwnu5coftehU0rTXupQvFLg/view?usp=sharing) or [Baidu Disk](https://pan.baidu.com/s/1PM0maLNCJxBsR9RIv18cTA)(4rgv). Then place the `Mixamo` directory within `retargeting/datasets`.

Otherwise, if you want to download Mixamo dataset or use your own dataset, please follow the instructions below. Unless specifically mentioned, all script should be run in `retargeting` directory.

- To download Mixamo on your own, you can refer to [this](https://github.com/ChrisWu1997/2D-Motion-Retargeting/blob/master/dataset/Guide%20For%20Downloading%20Mixamo%20Data.md) good tutorial. You will need to download as fbx file (skin is not required) and make a subdirectory for each character in `retargeting/datasets/Mixamo`. In our original implementation we download 60fps fbx files and downsample them into 30fps. Since we use an unpaired way in training, it is recommended to divide all motions into two equal size sets for each group and equal size sets for each character in each group. If you use your own data, you need to make sure that your dataset consists of bvh files with same t-pose. You should also put your dataset in subdirectories of `retargeting/datasets/Mixamo`.

- Enter `retargeting/datasets` directory and run `blender -b -P fbx2bvh.py` to convert fbx files to bvh files. If you already have bvh file as dataset, please skip this step.

- In our original implementation, we manually split three joints for skeletons in `group A`. If you want to follow our routine, run `python datasets/split_joint.py`. This step is optional.

- Run `python datasets/preprocess.py` to simplify the skeleton by removing some less interesting joints, e.g. fingers and convert bvh files into npy files. If you use your own data, you'll need to define simplified structure in `retargeting/datasets/bvh_parser.py`. This information currently is hard-coded in the code. See the comment in source file for more details. There are four steps to make your own dataset work.

- Training and testing character are hard-coded in `retargeting/datasets/__init__.py`. You'll need to modify it if you want to use your own dataset.

#### Train

After preparing dataset, simply run

```bash
cd retargeting
python train.py --save_dir=./training/
```

It will use default hyper-parameters to train the model and save trained model in `retargeting/training` directory. More options are available in `retargeting/option_parser.py`. You can use tensorboard to monitor the training progress by running

```bash
tensorboard --logdir=./retargeting/training/logs/
```
