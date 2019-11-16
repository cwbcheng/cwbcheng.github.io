# 目的
让ApolloWebUI项目支持CI/CD。其中，代码托管在GitHub上，部署在自己的云服务器上。
# CI
## 使用GitHub管理项目
## GitHub上安装Azure Pipelines应用
## 在dev.azure.com中添加Pipelines的Builds
Builds默认的步骤只有还原、编译和测试等步骤。为了支持CD，还需要添加.NET Core 的publish 和 Publish build artifacts 两个步骤。
# CD
## 在自己的CentOS上安装vsts agent
[官方安装文档](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops)
## dev.azure.com中添加Pipelines的Releases
因为我使用的是自己的环境，所以添加template的时候选择Empty Job。
Artifacts添加上面创建好的build pipeline，Stages的Agent Job中，Agent pool选择自己安装的agent所在的Agent pool。
我的项目里有三个Task：
* 解压CI过程生成的压缩包

    Extract files。要注意 Destination folder中填写的是解压的目标路径。
* 将压缩包的内容复制到指定目录

    使用Command line，脚本是
        
        cp -rf ./apolloui/* /home/username/dotnetcore/apolloui
        
* 重启应用的服务。

    同样使用Command line,脚本是        
            
        sudo systemctl  restart apolloui.service
    
    默认sudo 需要输入密码。修改sudoers文件，在最后一行添加
        
        username ALL=NOPASSWD:/usr/bin/systemctl

    使账号在执行systemctl 命令时不用输入密码。