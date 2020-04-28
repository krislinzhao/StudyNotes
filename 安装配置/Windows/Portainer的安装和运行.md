

# 1.创建容器卷

```bash
docker volume create portainer_data
```

# 2.安装Portainer

```bash
docker pull portainer/portainer
```

# 3.启动Portainer

```bash
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

