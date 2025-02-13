**常用的conda指令**

```bash
conda create -n env_name python=3.x # 创建新的Python环境
conda env list # 查看已有的Python环境
conda activate env_name # 进入已有的python环境
conda deactivate # 退出当前的Python环境
```

**常用的pip指令**

```bash
pip install -r requirements.txt # 安装依赖项

pip install package_name # 安装某个包名

pip install .... --timeout 6000 # 设置timeout
```

**pip 换清华源后缀:**

尾部加上

```bash
-i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
```

**指定地址下载torch**

```bash
pip install torch==1.6.0 -f https://download.pytorch.org/whl/torch_stable.html
```

**查看环境指令**

![image-20250122033806705](https://testingcf.jsdelivr.net/gh/Jokerealm/MyPic@img/img/image-20250122033806705.png)