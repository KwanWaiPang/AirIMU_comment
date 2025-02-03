[comment]: <> 

<!-- PROJECT LOGO -->

<p align="center">

  <h1 align="center"> AirIMU : Learning Uncertainty Propagation for Inertial Odometry
  </h1>

[comment]: <> (  <h2 align="center">PAPER</h2>)
  <h3 align="center">
  <a href="https://kwanwaipang.github.io/Blog_basedon_markdown/Deep-IMU-Bias/#airimu-learning-uncertainty-propagation-for-inertial-odometry">Blog</a> 
  | <a href="https://github.com/haleqiu/AirIMU">Original Github Page</a>
  </h3>
  <div align="justify">
  </div>

<br>

<!-- ~~~
rm -rf .git
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/KwanWaiPang/AirIMU_comment.git
git push -u origin main
~~~ -->

# 安装配置
1. 安装[pypose](https://github.com/pypose/pypose)
* pypose is a PyTorch-based library that combines deep perceptual models with physics-based optimization techniques.
```Bash
conda create -n airIMU python=3.10.11
conda activate airIMU
pip install pypose

# 安装系列依赖
pip install "numpy<2"
pip install tqdm
pip install pyproj
pip install scipy
pip install matplotlib
pip install pyhocon
pip install pykitti
pip install opencv-python==4.7.0.72

```

# 进行验证
1. 下载预训练模型（For EuRoC）
~~~
wget https://github.com/sleepycan/AirIMU/releases/download/pretrained_model_euroc/EuRoCWholeaug.zip
~~~

2. generate network inference file net_output.pickle（这应该是网络估算出来的IMU结果）
~~~
python inference.py --config configs/exp/EuRoC/codenet.conf
~~~
