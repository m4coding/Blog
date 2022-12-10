# Git科学上网 -- github

## window10 设置

配置ssh访问，也即是git clone git@github.com:xxxx这种

.ssh/config文件配置

    Host github.com
    User git
    Hostname ssh.github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    Port 22
    ProxyCommand connect -S 127.0.0.1:7890 %h %p
    
    macox上的配置
    # github
    Host github.com
    HostName ssh.github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_github
    Port 22
    ProxyCommand nc -v -x  127.0.0.1:1080 %h %p

## 参考

[git 设置和取消代理](https://gist.github.com/laispace/666dd7b27e9116faece6)
