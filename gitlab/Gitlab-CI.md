# Gitlab CI定制

为了代码提交的后可持续的处理操作，可以尝试用CI来处理

1、安装Gitlab-Runner

    # Linux x86-64
    sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
    sudo chmod +x /usr/local/bin/gitlab-runner
    
    # 添加一个系统用户 GitLab Runner 然后用于gitlab-runner，当然也可以用其他用户，如root用户（这样权限就会高点）
    sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
    sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
    
    # 启动服务
    sudo gitlab-runner start

2、Register Gitlab-Runner

    sudo gitlab-runner register
    然后输入对应的url和token

3、每个工程中添加的.gitlab-ci.yml就是对应的处理配置文件

## 一些坑

1、发现一些job被pending挂起，需要在gitlab对应的Runner中设置Indicates whether this runner can pick jobs without tags

2、发现unregister有异常，可以考虑sudo vi /etc/gitlab-runner/config.toml试试

## 参考

[Install GitLab Runner manually on GNU/Linux](https://docs.gitlab.com/runner/install/linux-manually.html)

[Registering runners](https://docs.gitlab.com/runner/register/index.html)

[Executors](https://docs.gitlab.com/runner/executors/README.html)

[使用GitLab实现CI/CD](https://zhuanlan.zhihu.com/p/136843588)

[Gitlab-CI使用教程](https://juejin.cn/post/6844904045581172744)

[JB的git之旅--.gitlab-ci.yml介绍](https://juejin.cn/post/6844903633998331918)

[JB的测试之旅-使用gitlab ci获取提交记录](https://blog.csdn.net/weixin_34405354/article/details/87994400)