## Linux添加cuda路径到环境变量

查看linux中存在的cuda版本，进入/usr/local：

![img](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/543473-20210827120949934-512093908.png)

添加自己需要的cuda版本到环境变量, vim ~/.bashrc，添加以下内容到最后，如：

```bash
export PATH=/usr/local/cuda-10.1/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64:$LD_LIBRARY_PATH
```

 然后source一下使其生效

```bash
source ~/.bashrc
nvcc -V
```

出现以下信息，则配置成功：

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Sun_Jul_28_19:07:16_PDT_2019
Cuda compilation tools, release 10.1, V10.1.243
```