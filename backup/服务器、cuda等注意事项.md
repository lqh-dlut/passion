通过`ls /usr/local/cuda-xxxx  `判断是否安装了xxxx版本的cuda

e.g. : 以下代码将通过修改环境变量切换cuda版本，**临时配置 CUDA 12.6 的环境变量**

`export PATH=/usr/local/cuda-12.6/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64:$LD_LIBRARY_PATH`

> 如果是长期使用，建议将配置写入 `~/.bashrc` 或 `~/.zshrc` 文件以实现永久生效。但是学校的服务器自己不一定有权限。



因此在使用学校的服务器时候，**cuda版本基本不能变动，需要找到对应的Pytorch版本**，如下安装兼容12.6cuda的pytorch

`pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126`



另外在下载大型数据集或者长期训练模型是需注意**使用screen命令**，防止中途断连很麻烦

- 启动：`screen -S <name>`
- 恢复：`screen -r <name>`
- 断开：`Ctrl + A` + `D`
- 终止：`exit` 或 `Ctrl + D`

进入滚动模式：`Ctrl + A` + `[` ，按 `Esc` 退出滚动模式。