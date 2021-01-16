## 安装Code-server

  brew install code-server
  brew services start code-server # Now visit http://127.0.0.1:8080. Your password is in ~/.config/code-server/config.yaml
  
为了通过ip地址访问，可以通过 例如： code-server --bind-addr 192.168.31.59:8080
