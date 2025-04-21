# frp-docker
run frpc/frps in docker.  
Fetch source code which is tagged as the latest from the GitHub repository named fatedier/frp， then build it and finally run frps/frpc on Alpine Linux.
## Run frps
docker build -t frps:latest .  
docker run -p 7000:7000 -d -v ./bin:/frp/bin frps:latest  
