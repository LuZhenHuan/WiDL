# Pytorch安装流程（初版）

Created: Sep 19, 2019 9:49 AM
Last Edited Time: Apr 29, 2020 10:29 AM
Status: Archived
Type: Technical Spec

## 基本环境安装

- Anaconda - 高质量的BLAS库（MKL），可控依赖版本。
- Python - 可通过Anaconda快速安装和切换
- CUDA - 服务器默认是CUDA 8.0，已安装
- 强烈建议在虚拟环境下安装pytorch和实验环境，方便管理和维护且不会修改掉系统环境。
- Anaconda安装步骤（必要）

```bash
# 安装anaconda（请确认好每一步的yes，安装完毕后重启终端让path生效）
# Athena
bash /media/huangcan/share/insatllpackages/pytorchEnvAthena/Anaconda3-5.3.1-Linux-x86_64.sh

# Gaea
bash /home/share/insatllpackages/pytorchEnvGaea/Anaconda3-5.3.1-Linux-x86_64.sh

# 建立新的虚拟环境，名称env_name替换为自订的名字，指定python版本为3.6
conda create -n env_name python=3.6
# 列出当前所有环境确认
conda env list
# 激活虚拟环境（优先尝试第一个，如果报错就尝试另一个命令）
source activate env_name
or
conda activate env_name
```

## 一、（主要-无需联网）硬拷贝虚拟环境

- 先替换pkg才能保证离线克隆环境不报错

```bash
# Athena
cp -r /media/huangcan/share/installpackage/pytorchEnvAthena/pkgs/* ~/anaconda3/pkgs/
conda create -n your_name --clone /media/huangcan/share/installpackage/pytorchEnvAthena/envs/torchEnv/ --offline

# gaea
cp -r /home/share/installpackage/pytorchEnvGaea/pkgs/* ~/anaconda3/pkgs/
conda create -n your_name --clone /home/share/installpackage/pytorchEnvGaea/envs/torchEnv/ --offline

****安装完成后见文档最后的验证方法****
```

## 二、（备选-需要联网）克隆虚拟环境

- 导入完整的安装环境配置

```bash
# 导入
conda env create -f testTorch.yaml

# 导出环境语句
# conda env export > py36.yaml1

****安装完成后见文档最后的验证方法****
```

## 三、（备选-需要联网）常规联网安装方法

- 更换清华源并安装CUDA8的pytorch

```bash
# 打开该文件添加内容
vim ~/.condarc

`
channels:
    - defaults
show_channel_urls: true           # 设置搜索时显示通道地址
default_channels:
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
custom_channels:
    conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
    simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
`
# 查看自己本机的cuda版本，注意大写
nvcc -V
# 安装pytorh，取决于你的cuda版本（替换为8.0、9.0、10.0等）,具体pytorch版本见官网
conda install pytorch==1.0.0 torchvision==0.2.1 cuda80 -c pytorch

****安装完成后见文档最后的验证方法****
```

## （备选-需要联网）源码编译方法

- 在虚拟环境下执行如下命令

```bash
# 清理
conda uninstall pytorch
# 查看cuda版本，确认为8.0（注意V为大写）
nvcc -V
# 安装必要库
conda install numpy pyyaml mkl=2019.3 mkl-include setuptools cmake cffi typing

# GPU对LAPACK的支持，暂时不用安装
# conda install -c pytorch magma-cuda80 # optional step

# 确认软链到cuda的目录，pytorch才能识别到cuda的版本
ls -ld /usr.local/cuda
# clone the pytorch source code
git clone --recursive https://github.com/pytorch/pytorch
cd pytorch
# make clean is needed in my case
make clean 
# 设置cmake
export CMAKE_PREFIX_PATH=${CONDA_PREFIX:-"$(dirname $(which conda))/../"}
python setup.py install

****安装完成后见文档最后的验证方法****
```

## 验证方法

- 进python导入成功即可

```bash
# 列出当前所有环境确认
conda env list
# 激活虚拟环境（优先尝试第一个，如果报错就尝试另一个命令）
source activate env_name
or
conda activate env_name
# 进入python测试
python
```

```python
import torch
print(torch.__version__)
```

## 附件

- conda虚拟环境相关操作（需要时再操作，现在无需操作）

```bash
# 确保Anaconda安装正确

# 建立新的虚拟环境，名称env_name替换为自订的名字，指定python版本为3.6
conda create -n env_name python=3.6
# 列出当前所有环境确认
conda env list
# 激活虚拟环境（优先尝试第一个，如果报错就尝试另一个命令）
source activate env_name
or
conda activate env_name
# 退出虚拟环境
conda deactivate env_name

# 删除虚拟环境
conda remove -n env_name --all

# 安装完毕（base）变为（env_name）代表激活成功
```

[yhjoker](https://www.cnblogs.com/yhjoker/p/10972795.html)

Pytorch 使用不同版本的 cuda, 确认本机cuda版本的几个渠道。