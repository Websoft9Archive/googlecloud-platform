# 连接Linux

连接Linux服务器一般有命令行和SFTP两种方式：

## 命令行连接

命令行（Command）是Linux系统基本的操作，AWS支持三种命令行连接方式：

| 方式                                                   | 操作说明                                                     |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| 一个独立的SSH客户端                                    | 需要下载 [putty](https://putty.org/) 等客户端到本地电脑来连接服务器 |
| 托管SSH客户端直接来自我的浏览器（Alpha）               | 从AWS控制台网页直接连接服务器，前置条件是服务器需要安装[EC2 Instance Connect](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/ec2-instance-connect-set-up.html) |
| 直接从我的浏览器连接的 Java SSH 客户端（需要安装Java） | 从AWS控制台网页直接连接服务器，前置条件是浏览器需要**安装Java插件** |


我们以 “**托管SSH客户端直接来自我们的浏览器**” 为例描述如何连接Linux

1. 参考[此处文档](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/ec2-instance-connect-set-up.html)，安装EC2 Instance Connect组件（Websoft9镜像默认已安装，忽略此步骤）

2. 登录AWS云控制台，打开：实例->连接，选择第二种连接方式后，点击“Connect”按钮
   ![命令行连接](https://libs.websoft9.com/Websoft9/DocsPicture/zh/aws/aws-connectmethods-websoft9.png)

3. 将打开一个窗口，并且您连接到实例。

通过命令行连接服务器之后，如下两个最常用的操作示例是需要掌握的：

### 示例1：获取数据库密码

为了安全考虑，用户每一次部署，都会生成唯一的随机数据库密码，存放在服务中。只需如下的一条命令，即可查看

```shell
sudo cat /credentials/password.txt

//运行结果
MySQL username:root
MySQL Password:@qDg1Vq1!V
```

### 示例2：启用系统root账号

AWS出于安全和法规要求，默认情况下没有开放Linux的root账号，只给用户提供了普通账号。如果您希望使用root账号，通过下面的步骤启用之：

```shell
sudo su
sudo sed -i 's/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo passwd root
```

## SFTP连接

SFTP是使用SSH协议的FTP模式，也称之为安全增强型的FTP。SFTP工具是Linux用户最喜欢的一种操作方式，下面以WinSCP这款SFTP工具为例，详细说明SFTP的使用。

### 配置WinSCP

1. 下载[WinSCP](https://winscp.net/) ，安装后，启动并新建一个连接
2. 根据云服务器的 **密码验证和秘钥对** 两种验证方式分别说明：
   - 密码验证方式设置（最常见的方式）
     ![密码验证方式](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/winscp-newsite.png)
   - 秘钥对验证方式设置
     ![秘钥对验证方式](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/winscp-secrets-websoft9.png)
3. 验证方式设置好之后，点击"登录"。登录中过程中，系统提示您是否保存登录信息，选择"是"
4. 成功连接后的界面
   ![WinSCP管理界面](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/websoft9-winscp-success.png)

### 管理文件

WinSCP 通过拖拽，就可以方便上传下载文件，可以对文件（夹）可以对进行多种设置与操作

1. 一般来说网站的文件都放在 */data/wwwroot* 目录下夹
   ![upload files](http://libs.websoft9.com/Websoft9/DocsPicture/en/winscp/winscp-dragfile-websoft9.png)

2. 右键单击服务器上一个文件或文件夹，可以对云服务器进行多种操作
   ![管理文件](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/websoft9-winscp-youjian.png)

3. 以修改文件权限为例的相关界面如下

   ![管理文件](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/websoft9-winscp-quanxian.png)

### 运行命令

WinSCP是自带命令运行功能的，虽然命令功能仅限于运行非交互式命名（即命令执行过程中无需反馈和过程中的输入），但对于初学者确简单实用。

1. WinSCP登录到服务器，点击菜单来的命令窗口图标（快捷键Ctrl+T也可以）
   ![命令行工具](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/winscp-ucmd-websoft9.png)
2. 在弹出的命令运行窗口执行命令（每次一条命令），以查询内存使用为例，运行命令 `free -m`
   ![命令行工具](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/wincp-showmemory-websoft9.png)

### 集成Putty

在某些特定的常见下，可能需要使用Putty来运行命令。由于Putty是一个命令操作界面，每次使用的时候都需要输入root密码，如果密码比较复杂，会让人感觉比较麻烦。其实WinSCP是可以集成Putty的，集成后，通过WinSCP就可以打开Putty，自动登录到服务器。

1. 打开Winscp-选项-集成-应用程序。Putty/terminal客户端路径这里为你本地putty.exe程序的路径
   ![命令行工具](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/websoft9-winscp-putty.png)
2. 集成成功后，只需要通过Winscp的窗口快捷方式即可打开Putty
   ![命令行工具](http://libs.websoft9.com/Websoft9/DocsPicture/zh/winscp/websoft9-winscp-puttyopen.png)

> 通过Winscp打开Putty操作与直接打开putty没有区别