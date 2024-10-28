1. 更新系统软件包
   sudo apt update

2. 安装依赖包【用于通过HTTPS来获取仓库】
   sudo apt install apt-transport-https ca-certificates curl software-properties-common

3. 添加Docker官方GPG密钥
   sudo -i
   curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-ce.gpg

4. 验证
   sudo apt-key fingerprint 0EBFCD88
   0EBFCD88 是公钥的指纹。执行这个命令后，系统会显示与该指纹相关的公钥信息。

4. 添加Docker阿里稳定版软件源
   sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

5. 再次更新软件包
   sudo apt update

6. 安装默认最新版
   sudo apt install docker-ce docker-ce-cli containerd.io

7. 测试，安装好后默认启动
   sudo docker run hello-world
   如果输出“Hello from Docker!”则表示Docker已经成功安装。

8. 查看有哪些镜像
   sudo docker images

9. 配置用户组
   sudo usermod -aG docker galaxfy
   su - galaxfy  # 刷新shell状态
   docker images # 验证

10. 其他docker运行命令
    查看状态：sudo systemctl status docker
    启动：sudo systemctl start docker
    开机自启：sudo systemctl enable docker
    停止：sudo systemctl stop docker

11. 配置docker镜像源
    sudo mkdir -p /etc/docker
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
     "registry-mirrors": ["请替换为您自己的代理服务ip或者域名"] 
    }
    EOF
    sudo systemctl daemon-reload
    sudo systemctl restart docker

12. 安装特定版docker
    sudo apt-cache madison docker-ce  # 显示可用版本
    sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io # 将需要的版本替换VERSION_STRING进行安装，例如 5:20.10.17~3-0~ubuntu-focal