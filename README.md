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
1. 下载预训练模型（For EuRoC）并放置于```experiments/EuRoC/codenet/ckpt/```
~~~
wget https://github.com/sleepycan/AirIMU/releases/download/pretrained_model_euroc/EuRoCWholeaug.zip
~~~

2. 下载数据集[Euroc](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets)：**Note**: Remember to reset the `data_root` in `configs/datasets/${DATASET}/${DATASET}.conf`.

3. generate network inference file net_output.pickle（这应该是网络估算出来的IMU结果）
~~~
python inference.py --config configs/exp/EuRoC/codenet.conf --load  EuRoCWholeaug/ckpt/best_model.ckpt
~~~
网络的输出（应该也就是每个序列的网络估算的IMU以及协方差）保存出来
<div align="center">
  <img src="./微信截图_20250203155329.png" width="80%" />
<figcaption>  
</figcaption>
</div>

4. 基于网络输出的结果来验证系统的系统
~~~
python evaluation/evaluate_state.py --dataconf configs/datasets/BaselineEuroc/Euroc_1000.conf --exp experiments/EuRoC/codenet/
~~~

5. 结果如下：
* MH_02_easy
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./微信截图_20250203162610.png" width="100%" />
      </td>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/MH_02_easy_orientation_compare.png" width="100%" />
      </td>
    </tr>
    <tr>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/MH_02_easyinte_error_compare.png" width="100%" />
      </td>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/MH_02_easystate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* MH_04_difficult
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./微信截图_20250203162640.png" width="100%" />
      </td>
       <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/MH_04_difficult_orientation_compare.png" width="100%" />
      </td>
    </tr>
     <tr>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/MH_04_difficultinte_error_compare.png" width="100%" />
      </td>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/MH_04_difficultstate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* V1_03_difficult
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./微信截图_20250203163931.png" width="100%" />
      </td>
       <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_03_difficult_orientation_compare.png" width="100%" />
      </td>
    </tr>
     <tr>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_03_difficultinte_error_compare.png" width="100%" />
      </td>
      <td style="width: 50%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_03_difficultstate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* V1_01_easy
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_01_easy_orientation_compare.png" width="100%" />
      </td>
       <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_01_easyinte_error_compare.png" width="100%" />
      </td>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_01_easystate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* V1_02_medium
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_02_medium_orientation_compare.png" width="100%" />
      </td>
       <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_02_mediuminte_error_compare.png" width="100%" />
      </td>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_02_mediumstate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* V1_03_difficult
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_03_difficult_orientation_compare.png" width="100%" />
      </td>
       <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_03_difficultinte_error_compare.png" width="100%" />
      </td>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V1_03_difficultstate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* V2_01_easy
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V2_01_easy_orientation_compare.png" width="100%" />
      </td>
       <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V2_01_easyinte_error_compare.png" width="100%" />
      </td>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V2_01_easystate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>

* V2_02_medium
<div align="center">
  <table style="border: none; background-color: transparent;">
    <tr>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V2_02_medium_orientation_compare.png" width="100%" />
      </td>
       <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V2_02_mediuminte_error_compare.png" width="100%" />
      </td>
      <td style="width: 30%; border: none; padding: 0.01; background-color: transparent; vertical-align: middle;">
        <img src="./result/loss_result/V2_02_mediumstate_error_compare.png" width="100%" />
      </td>
    </tr>
  </table>
  <figcaption>
  </figcaption>
</div>
