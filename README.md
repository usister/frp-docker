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
