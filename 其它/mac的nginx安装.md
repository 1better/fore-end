## mac中nginx以及echo模块的安装

> ```shell
> # 不要编译安装 （各种出错，直接用homebrew安装快并且简洁）
> # 操作顺序
> 
> brew tap denji/homebrew-nginx (克隆到本地)
> 
> brew unlink nginx（之前安装过nginx需要这一步）
> 
> brew install nginx-full --with-rtmp-module (后边跟模块名字)
> 
> nginx (启动)
> ```

> github     https://github.com/Homebrew/homebrew-nginx （查看包含的模块）

## nginx常用的指令

> ```shell
> # 开启nginx (sudo强制)
> sudo nginx 
> # 关闭
> sudo nginx -s stop
> # 指定conf
> sudo nginx -c +路径
> # 重新加载
> sudo nginx -s reload
> 
> # 看版本号
> nginx -v
> # 查看详细信息，包括编译参数
> nginx -V
> ```
>
>  

## 查看log日志

```js
# 创建logs文件夹 每次运行报错都会在logs的error.log都会显示日志

error_log  /usr/local/etc/logs/error.log;
```

