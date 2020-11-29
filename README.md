# Description
This repository shows how to reproduce an error on Linux that appears when rebuilding [E2X][1] as stated in their [repository][3].  
The build was done for Python 3, CPU-only.  
The `ssd` branch contains an already merged version of the [caffe/ssd][2] branch and the [E2X repository][3] with every merge conflict being resolved in favor of the latter.

# Steps to reproduce
I followed [this manual][4] to install the dependencies for Caffe.  
However, the part about customizing `/etc/apt/sources.list` from [this page][5] didn't work, so I used the packages from the part [For Ubuntu (< 17.04)][4].  

To reproduce the error, use the following commands to install the dependencies:  
```
sudo apt-get update
sudo apt-get install cmake
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install python-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```
In the directory of this repository:  
```
mkdir build
cd build
cmake ..
make all
```
On the 20th percent, you will get the following error from `make`:
```
detection_evaluate_layer.cpp: In member function ‘virtual void caffe::DetectionEvaluateLayer<Dtype>::Forward_cpu(const std::vector<caffe::Blob<Dtype>*>&, const std::vector<caffe::Blob<Dtype>*>&)’:
detection_evaluate_layer.cpp:170:58: error: ‘const value_type’ {aka ‘const class caffe::NormalizedBBox’} has no member named ‘idx’
  170 |           top_data[num_det * EVAL_SHAPE + 9] = bboxes[i].idx();
      |                                                          ^~~
detection_evaluate_layer.cpp:196:60: error: ‘__gnu_cxx::__alloc_traits<std::allocator<caffe::NormalizedBBox>, caffe::NormalizedBBox>::value_type’ {aka ‘class caffe::NormalizedBBox’} has no member named ‘idx’
  196 |             top_data[num_det * EVAL_SHAPE + 9] = bboxes[i].idx();
      |                                                            ^~~
detection_evaluate_layer.cpp:221:60: error: ‘__gnu_cxx::__alloc_traits<std::allocator<caffe::NormalizedBBox>, caffe::NormalizedBBox>::value_type’ {aka ‘class caffe::NormalizedBBox’} has no member named ‘idx’
  221 |             top_data[num_det * EVAL_SHAPE + 9] = bboxes[i].idx();
      |                                                            ^~~
```
The function `.idx()` has not been used by the original Caffe distribution.  
If anybody knows how to fix this error, feel free to contact me through issues or by email dmitriybadretdinov@gmail.com.

[1]: http://arxiv.org/abs/1811.08011
[2]: https://github.com/weiliu89/caffe/tree/ssd
[3]: https://github.com/gudovskiy/e2x
[4]: https://caffe.berkeleyvision.org/install_apt.html
[5]: https://caffe.berkeleyvision.org/install_apt_debian.html
