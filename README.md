# vid2depth_modifications
Modified version of tensorflow's vid2depth software. Updated to (hopefully) run without bugs.  
All modified code is in the vid2depth folder.

### ORIGINAL: https://github.com/tensorflow/models/tree/master/research/vid2depth

### DEPENDENCIES:

pip install absl-py  
pip install matplotlib  
pip install numpy  
pip install scipy  
pip install tensorflow  


### Specifications (these worked but others might work as well):

Python        3.5  
absl-py       0.7.1  
matplotlib    3.0.3  
numpy         1.16.4  
scipy         1.1.0  
tensorflow    1.5.0  


### TRAINING:

a) Use the included model pre-trained on KITTI and Cityscapes data. This option requires no further action.

b) Train your own model:
    This is more work. 
    
### Format training data:

Create folder kitti-raw-uncompressed in /vid2depth  
Move all raw input data into /vid2depth/kitti-raw-uncompressed  

The data structure should appear as follows: 
```
kitti-raw-uncompressed  
	| --- video_0  // these video names should all be in dataset_loader / KittiRaw.date_list  
	| --- video_1  
	…  
	| ---video_n  
        | --- calib_cam_to_cam.txt  
		| --- subfolder_0  
		| --- subfolder_1  // optional subdivisions of video n  
		…  
		| --- subfolder_n   
            | --- image_00  // the number after the underscore should be the same as in   
		                       dataset_loader ---> KittiRaw.cam_ids  
	            | --- data  
		            | --- all images in this folder  
```
calib_cam_to_cam.txt contains the camera intrinsics for the given video 
From the original code:  

"calib_cam_to_cam.txt: Camera-to-camera calibration  

  - S_xx: 1x2 size of image xx before rectification  
  - K_xx: 3x3 calibration matrix of camera xx before rectification  
  - D_xx: 1x5 distortion vector of camera xx before rectification  
  - R_xx: 3x3 rotation matrix of camera xx (extrinsic)  
  - T_xx: 3x1 translation vector of camera xx (extrinsic)  
  - S_rect_xx: 1x2 size of image xx after rectification  
  - R_rect_xx: 3x3 rectifying rotation to make image planes co-planar  
  - P_rect_xx: 3x4 projection matrix after rectification  

Note: When using this dataset you will most likely need to access only  
P_rect_xx, as this matrix is valid for the rectified image sequences."

Example calib_cam_to_cam.txt:  
```
calib_time: 09-Jan-2012 13:57:47  
corner_dist: 9.950000e-02  
S_00: 1.392000e+03 5.120000e+02  
K_00: 9.842439e+02 0.000000e+00 6.900000e+02 0.000000e+00 9.808141e+02 2.331966e+02 0.000000e+00 0.000000e+00 1.000000e+00  
D_00: -3.728755e-01 2.037299e-01 2.219027e-03 1.383707e-03 -7.233722e-02  
R_00: 1.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 1.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 1.000000e+00  
T_00: 2.573699e-16 -1.059758e-16 1.614870e-16  
S_rect_00: 1.242000e+03 3.750000e+02  
R_rect_00: 9.999239e-01 9.837760e-03 -7.445048e-03 -9.869795e-03 9.999421e-01 -4.278459e-03 7.402527e-03 4.351614e-03 9.999631e-01  
P_rect_00: 7.215377e+02 0.000000e+00 6.095593e+02 0.000000e+00 0.000000e+00 7.215377e+02 1.728540e+02 0.000000e+00   0.000000e+00 0.000000e+00 1.000000e+00 0.000000e+00  
```
Run the following command

cd path/to/vid2depth    
python3 dataset/gen_data.py \\  
  --dataset_name kitti_raw_eigen \\  
  --dataset_dir kitti-raw-uncompressed \\  
  --data_dir data/kitti_raw_eigen \\  
  --seq_length 3

### Train model (skip if using pre-trained model)

python3 train.py \\  
  --logtostderr \\  
  --data_dir data/kitti-raw-eigen \\  
  --seq_length 3 \\  
  --reconstr_weight 0.85 \\  
  --smooth_weight 0.05 \\  
  --ssim_weight 0.15 \\  
  --icp_weight 0 \\  
  --checkpoint_dir checkpoints 


### USAGE:

To get depth maps from images:

cd path/to/vid2depth  
python3 inference.py \\  
    --kitti_dir /first/half/of/path/to/image/directory \\  
    --output_dir /path/to/where/you/want/outputs/stored \\  
    --kitti_video /second/half/of/path/to/image/directory \\  
    --model_ckpt /path/to/trained/model

Parameters kitti_dir and kitti_video are somewhat redundant; the two of them combined should make up the pathname to the directory where your data are stored.

Example usage (after creating /PNGs/ folder with input data in vid2depth/inference_data and /out/ folder in /vid2depth/):

cd ~/git/SfM/models/research/vid2depth  
python3 inference.py \\  
    --kitti_dir /inference_data \\  
    --output_dir /out \\  
    --kitti_video /PNGs \\  
    --model_ckpt trained-model/model_model-119496/model-119496
