常用命令：
```sh
# gulp
docker run --rm   -v E:\codes\esop:/data fangdany/node-futu sh -c "npm install;gulp"
docker run --rm   -v E:\codes\esop-support:/data  fangdany/node-futu sh -c "npm install;gulp"
docker run --rm   -v E:\codes\futu5\source\frontend:/data fangdany/node-futu sh -c "npm install;gulp"

# all
docker run -it --rm --name build -v E:\codes:/data fangdany/node-futu bash

# portainer
docker run -d --restart=always -p 9703:9000 -name portainer -v /var/run/docker.sock:/var/run/docker.sock -v portainer:/data portainer/portainer
```
