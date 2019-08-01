# vid2depth_modifications
Modified version of tensorflow's vid2depth software. Updated to (hopefully) run without bugs

Usage:

To get depth maps from images:

cd path/to/vid2depth
python3 inference.py \
    --kitti_dir /first/half/of/path/to/image/directory \
    --output_dir /path/to/where/you/want/outputs/stored \
    --kitti_video /second/half/of/path/to/image/directory \
    --model_ckpt /path/to/trained/model

Parameters kitti_dir and kitti_video are somewhat redundant; the two of them combined should make up the pathname to the directory where your data are stored.

Example usage (after creating /out/ folder in /vid2depth/):

cd ~/git/SfM/models/research/vid2depth 
python3 inference.py \
    --kitti_dir /kitti-raw-uncompressed \
    --output_dir /out/PNG_out \
    --kitti_video /PNGs \
    --model_ckpt trained-model/model-119496/model-119496
