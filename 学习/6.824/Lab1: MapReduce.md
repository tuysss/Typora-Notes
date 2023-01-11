## 1. go环境配置和goland

1. goland破解：https://www.bilibili.com/read/cv11179149

2. clone lab后，关于测试用例跑不通的问题解决

   解决go配置问题：

   第一步，

   ```shell 
   ../mrapps/wc.go:9:8: "../mr" is relative, but relative import paths are not supported in module mode
   ```

   实质上是go module新的包管理和老的GOPATH管理冲突问题

   参考：https://juejin.cn/post/7018756637330685988

   第二步，

   解决主要问题后，发现仍存在GOROOT（即go的安装路径）配置错误，改过来就好了.

   因为homebrew安装的，都在`/usr/local/Cellar/`目录下

   



