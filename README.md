# frp-docker
run frpc/frps in docker.  
Fetch source code which is tagged as the latest from the GitHub repository named fatedier/frp， then build it and finally run frps/frpc on Alpine Linux.
## Run frps
```shell
git clone https://github.com/usister/frp-docker.git  
cd frp-docker/frps-docker  
sudo docker build -t frps:latest .  
# Change the ports and the your_config_file_path to your's
sudo docker run -p 7000:7000 -d -v your_config_file_path:/etc/frp/frps.toml frps:latest --network host  
```
## Run frpc
```shell
git clone https://github.com/usister/frp-docker.git  
cd frp-docker/frpc-docker  
sudo docker build -t frpc:latest .
# Change the ports and the your_config_file_path to your's
sudo docker run -d -v your_config_file:/etc/frp/frpc.toml frpc:latest --network host  
```
## Fully frp toml config reference
Vist https://gofrp.org/en/docs/  
## Fully docker run command reference
Vist https://docs.docker.com/reference/cli/docker/container/run/  


# Example
## Expose web service by domain
### Install and configure frps
```shell
# Clone source code
git clone https://github.com/usister/frp-docker.git
# Compile
cd frp-docker/frps-docker  
sudo docker build -t frps:latest .
# Generate frps config file
# You need to change the token, and this token must same as fprc.toml
sudo mkdir /etc/frp
sudo bash -c 'cat << EOF > /etc/frp/frps.toml
# The port on your FRPS server that listens for FRPC connections.
bindPort = 7000
# The port on your FRPS server that exposes web server running on the FRPC side.
vhostHTTPPort = 80
[auth]
token = "change_me"
EOF'
# Run container
# Because of Docker's network policy, you must either expose all necessary ports or use the host network. Using the host network is recommended.
# sudo docker run -p 7000:7000 -p 80:80 -d -v /etc/frp/frps.toml:/etc/frp/frps.toml --restart unless-stopped --name frps  frps:latest
sudo docker run --network host -d -v /etc/frp/frps.toml:/etc/frp/frps.toml --restart unless-stopped --name frps  frps:latest
```
### Install and configure frpc
```shell
# Clone source code
git clone https://github.com/usister/frp-docker.git
# Compile
cd frp-docker/frpc-docker  
sudo docker build -t frpc:latest .
# Generate frps config file
sudo mkdir /etc/frp
sudo bash -c 'cat << EOF > /etc/frp/frpc.toml
# Your FRPS server ip and port
serverAddr = "x.x.x.x"
serverPort = 7000

[auth]
token = "change_me"

[[proxies]]
name = "web"
type = "http"
localPort = 80
customDomains = ["www.yourdomain.com"]

[[proxies]]
name = "web2"
type = "http"
localPort = 8080
customDomains = ["www.yourdomain2.com"]

EOF'
# Run container
# Due to docker network security policy, you need to configure FRPC container and your web services container in the same network, or configure FRPC container in host network. This example uses host network.
sudo docker run -d -v /etc/frp/frpc.toml:/etc/frp/frpc.toml --network host --restart unless-stopped --name frpc frpc:latest
```
