安装docker
`sudo apt install docker.io -y`

配置docker用户权限
`sudo groupadd docker`
`sudo gpasswd -a $USER docker`
`newgrp docker`
`sudo chmod a+rw /var/run/docker.sock`