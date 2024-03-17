# cloudflare-docker-proxy

![deploy](https://github.com/ciiiii/cloudflare-docker-proxy/actions/workflows/deploy.yaml/badge.svg)

> If you're looking for proxy for helm, maybe you can try [cloudflare-helm-proxy](https://github.com/ciiiii/cloudflare-helm-proxy).

## Deploy
[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

1. fork this project
2. modify the link of the above button to your fork url
3. click the button, you will be redirected to the deploy page

## Config tutorial

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
     "docker.libcuda.so": "https://registry-1.docker.io",
     "quay.libcuda.so": "https://quay.io",
     "gcr.libcuda.so": "https://k8s.gcr.io",
     "k8s-gcr.libcuda.so": "https://k8s.gcr.io",
     "ghcr.libcuda.so": "https://ghcr.io",
   };
   ```


## Config Docker mirrors
1. modify `/etc/docker/daemon.json`, add **registry-mirrors** as follow:
   - if you choose **4.1 use cloudflare worker host**.   
     Change the following **url** to your **actually url** (which you can find in **Preview** or **Routes** of your worker).
    ```javascript
    {
        "registry-mirrors": ["https://<workername>.<username>.workers.dev"]
    }
    ```
    - if you choose **4.2 use custom domain**
    ```javascript
    {
        "registry-mirrors": ["https://docker.yourdomain.com"]
    }
    ```
2. restart docker daemon
```
   systemctl restart docker.service
```
   

