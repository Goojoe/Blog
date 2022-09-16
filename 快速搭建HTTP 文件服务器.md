# 快速搭建HTTP 文件服务器

## [GoHttpServer](https://github.com/codeskyblue/gohttpserver)

## Docker用法

共享当前目录

```
docker run -it --rm -p 8000:8000 -v $PWD:/app/public --name gohttpserver codeskyblue/gohttpserver
```

