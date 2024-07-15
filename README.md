# cloudflare-docker-proxy

![deploy](https://github.com/ciiiii/cloudflare-docker-proxy/actions/workflows/deploy.yaml/badge.svg)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/pawpaw2022/cloudflare-docker-proxy)

> If you're looking for proxy for helm, maybe you can try [cloudflare-helm-proxy](https://github.com/ciiiii/cloudflare-helm-proxy).

## Deploy

1. click the "Deploy With Workers" button
2. follow the instructions to fork and deploy
3. update routes as you requirement

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/pawpaw2022/cloudflare-docker-proxy)

## Routes configuration tutorial

1. use cloudflare worker host: only support proxy one registry
   ```javascript
   const routes = {
     "${workername}.${username}.workers.dev/": "https://registry-1.docker.io",
   };
   ```
2. use custom domain: support proxy multiple registries route by host
   - host your domain DNS on cloudflare
   - add `A` record of xxx.example.com to `192.0.2.1`
   - deploy this project to cloudflare workers
   - add `xxx.example.com/*` to HTTP routes of workers
   - add more records and modify the config as you need
   ```javascript
   const routes = {
     "docker.pawpaw2022.com": "https://registry-1.docker.io",
     "quay.pawpaw2022.com": "https://quay.io",
     "gcr.pawpaw2022.com": "https://k8s.gcr.io",
     "k8s-gcr.pawpaw2022.com": "https://k8s.gcr.io",
     "ghcr.pawpaw2022.com": "https://ghcr.io",
   };
   ```


---


# Docker 镜像加速

自用 Docker Hub 镜像加速: https://appscross.com

为了加速镜像拉取，使用以下命令设置 registry mirror:

```bash
# sudo if needed 

mkdir -p /etc/docker

tee /etc/docker/daemon.json <<EOF
{
    "registry-mirrors": ["https://docker.pawpaw2022.com"]
}
EOF

systemctl daemon-reload

systemctl restart docker
```

不用设置环境也可以直接使用，用法示例：


```bash
docker pull docker.pawpaw2022.com/library/mysql:8.0
```

说明：library 是一个特殊的命名空间，它代表的是官方镜像。


自建镜像，请勿滥用。
