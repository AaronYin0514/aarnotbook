# Docker

## 修改配置

随便进一个容器

```
docker run -it --rm --privileged --pid=host justincormack/nsenter1
```

然后修改配置

```shell
cd /var/lib/docker/containers/f992ccd297****
vi hostconfig.json
```

