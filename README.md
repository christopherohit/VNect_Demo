# VNect

A tensorflow implementation of [VNect: Real-time 3D Human Pose Estimation with a Single RGB Camera](http://gvv.mpi-inf.mpg.de/projects/VNect/).

For the caffe model/weights required in the repository: please contact [the author of the paper](http://gvv.mpi-inf.mpg.de/projects/VNect/).

<p align="center">
    <img src="./pic/test_pic_show.png" height="260">
</p>
<p align="center">
    <img src="./pic/test_video_show.gif" height="300">
</p>


## Environments

- Python 3.x
- tensorflow-gpu 1.x
- [pycaffe](https://github.com/BVLC/caffe/tree/windows)

## Setup

<details><summary>Fedora 29</summary>
<p>

#### Install python dependencies:
```
pip3 install -r requirements.txt --user
```
#### Install caffe dependencies
```
sudo dnf install protobuf-devel leveldb-devel snappy-devel opencv-devel boost-devel hdf5-devel glog-devel gflags-devel lmdb-devel atlas-devel python-lxml boost-python3-devel
```
#### Setup Caffe
```
git clone https://github.com/BVLC/caffe.git
cd caffe
```

#### Configure Makefile.config (Include python3 and fix path)

#### Build Caffe
```
sudo make all
sudo make runtest
sudo make pycaffe
sudo make distribute
sudo cp .build_release/lib/ /usr/lib64
sudo cp -a distribute/python/caffe/ /usr/lib/python3.7/site-packages/
```
</p>
</details>


## Usage

### Preparation

1. Drop the pretrained caffe model into `models/caffe_model`.
2. Run `init_weights.py` to generate tensorflow model.

### Application

1. `run_estimator.py` is a script for **video stream**.
2. **(Recommended)** `run_estimator_ps.py` is a multiprocessing version script. When 3d plotting function shuts down in `run_estimator.py` mentioned above, you can try this one.
3. `run_pic.py` is a script for **picture**.
4. **(Deprecated)** `benchmark.py` is a class implementation containing all the elements needed to run the model.
5. **(Deprecated)** `run_estimator_robot.py` additionally provides ROS network and/or serial connection for communication in robot controlling.
6. **(Deprecated)** The training script `train.py` is not complete yet (I failed to reconstruct the model: ( So do not use it. Also pulling requests are welcomed.

**[Tips]** To run the scripts for video stream:

1.  click left mouse button to initialize the bounding box implemented by a simple HOG method;

2. trigger any keyboard input to exit while running.

## Notes

1. With some certain programming environments, the 3d plotting function (from matplotlib) in `run_estimator.py` shuts down. Use `run_estimator_ps.py` instead.
2. The input image is in BGR color format and the pixel value is mapped into a range of [-0.4, 0.6).
3. The joint-parent map (detailed information in `materials/joint_index.xlsx`):

<p align="center">
    <img src="./pic/joint_index.png" height="300">
</p>

3. Here I have a sketch to show the joint positions (don't laugh lol):

<p align="center">
    <img src="./pic/joint_pos.jpg" height="300">
</p>

4. Every input image is assumed to contain 21 joints to be found, which means it is easy to fit wrong results when a joint is actually not in the picture.

## About Training Data

For MPI-INF-3DHP dataset, refer to [my another repository](https://github.com/XinArkh/mpi_inf_3dhp).

## Reference Repositories

- original MATLAB implementation provided by the paper author.
- [timctho/VNect-tensorflow](https://github.com/timctho/VNect-tensorflow)
- [EJShim/vnect_estimator](https://github.com/EJShim/vnect_estimator)
