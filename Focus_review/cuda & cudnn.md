# centos下搭配一下tensorflow-gpu  

1. 官网下载cuda8.0.run和cudnn.tgz, centos图形化界面 sudu sh \**.run 出现错误，需要关闭 X service。  
2. 执行，注销当前用户，按ctrl+alt+f2，进入纯命令行模式，写入root，plus密码，进入安装包所在位置，执行安装。run命令。
3. 解压cudnn， 将文件复制至 cuda目录下。（cuda默认/usr/local/cuda-8.0）
