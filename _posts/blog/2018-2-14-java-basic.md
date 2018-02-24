---
layout: post
title: java面试基础内容
categories: Blog
description: 没有靠谱描述
keywords: java
---
java面试会考察的基础知识点

# 语言知识
* Java是否存在内存泄漏的问题

```shell
1.静态集合类。比如HashMap这类数据结构，如果这些容器是静态的，由于他们的生命周期和程序一致，所以容器内的对象在容器的生命周期结束之前不会被释放。
2.各种连接。比如数据库、网络等等。如果访问数据库的过程中对Connection、Statement等不显式关闭，将会造成大量的对象无法被回收，从而引起内存泄漏。
3.


```


### cuda的apt-get安装，版本8.0
```shell
sudo apt-get update && sudo apt-get install wget -y --no-install-recommends
wget "http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.61-1_amd64.deb"
sudo dpkg -i cuda-repo-ubuntu1604_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
``` 
### cudnn的安装，版本号为5.1
```shell
CUDNN_URL="http://developer.download.nvidia.com/compute/redist/cudnn/v5.1/cudnn-8.0-linux-x64-v5.1.tgz"
wget ${CUDNN_URL}
sudo tar -xzf cudnn-8.0-linux-x64-v5.1.tgz -C /usr/local
rm cudnn-8.0-linux-x64-v5.1.tgz && sudo ldconfig
```

## caffe2需要的其他依赖
同样的，还需要一些caffe2自身依赖的库。这部分可选，根据库名可以看出这里面包含opencv这样的图像处理的库，leveldb这样的文件格式处理库，openmpi这样的mpi通讯框架（用于分布式训练）等等，以及一些python上可选的依赖。

```shell
# for both Ubuntu 14.04 and 16.04
sudo apt-get install -y --no-install-recommends \
      libgtest-dev \
      libiomp-dev \
      libleveldb-dev \
      liblmdb-dev \
      libopencv-dev \
      libopenmpi-dev \
      libsnappy-dev \
      openmpi-bin \
      openmpi-doc \
      python-pydot
sudo pip install \
      flask \
      future \
      graphviz \
      hypothesis \
      jupyter \
      matplotlib \
      pydot python-nvd3 \
      pyyaml \
      requests \
      scikit-image \
      scipy \
      setuptools \
      six \
      tornado
```
## 编译安装
clone项目并且编译，clone的速度可能不会很快，因为还需要下载一些第三方的依赖文件，花费时间比较长。
```shell
git clone --recursive https://github.com/caffe2/caffe2.git && cd caffe2
make && cd build && sudo make install
python -c 'from caffe2.python import core' 2>/dev/null && echo "Success" || echo "Failure"
```
上面给出了编译之后测试是否成功的方法，在python中导入相应的包看看是否成功。
```shell
python -m caffe2.python.operator_test.relu_op_test
```
执行以上的python模块化执行的命令可以测试是否能够正常地在caffe2中使用GPU，需要注意的是此时不能更改目录，还需要在make时候进入的build目录中。
为了能够在所有目录中都能正常使用caffe2，需要修改环境变量。建议修改用户目录下的.bashrc文件，这样系统的不同用户之间不会冲突。
```shell
vim ~/.bashrc
#在文件的最后一行插入以下内容(需要替换caffe2的路径)
export PYTHONPATH=/usr/local:$PYTHONPATH
export PYTHONPATH=$PYTHONPATH:/path/to/caffe2_ROOT/build

export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

#然后保存文件重新source一下
source ~/.bashrc
```



