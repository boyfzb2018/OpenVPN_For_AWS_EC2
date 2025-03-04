### 在AWS EC2 Ubuntu实例上安装OpenVPN支持ClientVPN访问

1. **创建EC2实例**  
   首先，在与目标服务器相同的VPC中创建一个EC2实例，并将其放置在公共子网中。同时，为该实例附加一个弹性IP。创建一个新的安全组，并添加允许入站https-443、ssh-22、udp-1194的3条入站规则。

2. **更新EC2实例**  
   连接到EC2实例，运行以下命令以更新系统：  
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```

3. **下载并安装OpenVPN脚本**  
   使用`curl`命令下载OpenVPN安装脚本，并使用`chmod`命令使其可执行：  
   ```bash
   curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
   chmod +x openvpn-install.sh
   ```


4. **运行安装脚本**  
   执行安装脚本，并根据提示配置OpenVPN服务器。注意：在第一个问题中，输入EC2实例的公共IP。  
   ```bash
   sudo bash openvpn-install.sh
   ```

5. **确认OpenVPN服务状态**  
   安装完成后，确认OpenVPN服务正在运行：  
   ```bash
   sudo systemctl status openvpn
   ```

6. **确认OpenVPN监听端口**  
   确认OpenVPN守护进程正在监听指定端口：  
   ```bash
   sudo ss -tupln | grep openvpn
   ```

7. **复制客户端配置文件到本地**  
   将生成的客户端配置文件复制到本地机器（在本地机器上运行以下命令）：  
    ```bash
    scp -i "mykeypairppk.pem" ubuntu@ec2-52-83-193-39.cn-northwest-1.compute.amazonaws.com.cn:/home/ubuntu/myvpn.ovpn /Users/MichaelXu/Documents/nwcd/keypair
    ```

8. **在本地机器（macOS或windows）上配置OpenVPN**  
   - [下载并安装OpenVPN Connect客户端。](https://openvpn.net/client/client-connect-vpn-for-windows/) 
   - 安装完成后，选择导入配置文件，浏览从EC2实例复制的文件，并输入密码（建议保存密码以便下次使用）。  
   - 连接VPN后，即可访问目标服务器。

9. **管理OpenVPN用户**  
   如果需要添加新用户、撤销现有用户或删除OpenVPN服务器，只需再次运行安装脚本，并根据提示操作：
![1F1BC458-C521-4920-84AE-5B79C3E310AC](https://github.com/user-attachments/assets/f8830fe0-5ec7-41f0-9f6a-b7e800b93609)
  
   ```bash
   sudo bash openvpn-install.sh
   ```
