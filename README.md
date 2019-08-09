# vid2depth_modifications
Modified version of tensorflow's vid2depth software. Updated to (hopefully) run without bugs.  
All modified code is in the vid2depth folder.

ORIGINAL: https://github.com/tensorflow/models/tree/master/research/vid2depth

DEPENDENCIES:

pip install absl-py  
pip install matplotlib  
pip install numpy  
pip install scipy  
pip install tensorflow  

Specifications (these worked but others might work as well):
Python        3.5  
absl-py       0.7.1  
matplotlib    3.0.3  
numpy         1.16.4  
scipy         1.1.0  
tensorflow    1.5.0  


TRAINING:

a) Use the included model pre-trained on KITTI and Cityscapes data. This option requires no further action.

b) Train your own model:
    This is more work. 
    
Format training data:

Create folder kitti-raw-uncompressed in /vid2depth  
Move all raw input data into /vid2depth/kitti-raw-uncompressed  
Run the following command

cd path/to/vid2depth \
python3 dataset/gen_data.py \
  --dataset_name kitti_raw_eigen \
  --dataset_dir kitti-raw-uncompressed \
  --data_dir data/kitti_raw_eigen \
  --seq_length 3


USAGE:

To get depth maps from images:

cd path/to/vid2depth  
python3 inference.py \\
    --kitti_dir /first/half/of/path/to/image/directory \\
    --output_dir /path/to/where/you/want/outputs/stored \\
    --kitti_video /second/half/of/path/to/image/directory \\
    --model_ckpt /path/to/trained/model

Parameters kitti_dir and kitti_video are somewhat redundant; the two of them combined should make up the pathname to the directory where your data are stored.

Example usage (after creating /PNGs/ folder with input data in /kitti-raw-uncompressed and /out/ folder in /vid2depth/):

cd ~/git/SfM/models/research/vid2depth 
python3 inference.py \\
    --kitti_dir /kitti-raw-uncompressed \\
    --output_dir /out \\
    --kitti_video /PNGs \\
    --model_ckpt trained-model/model_model-119496/model-119496
