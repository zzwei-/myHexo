title: 使用hexo搭建github.io博客
date: 2015-07-01 12:31:39
tags: 杂七杂八
---
记录一下使用hexo搭建博客的步骤，否则长时间不使用hexo的话就会忘记。

1.安装node.js和git，拥有github账号。  
2.安装hexo。  
3.创建本地hexo文件夹，初始化:`hexo init`。  
4.安装依赖:`npm install`。  
5.完成本地安装:`hexo g`,`hexo s`;打开浏览器`127.0.0.1:4000`可以看到本地生成的网页内容。  
6.在github上创建一个name格式为 `'用户名'.github.io`的仓库，生成测试页面并发步。  
7.创建SSH keys，并添加到自己的github仓库里，测试是否连接成功。  
8.配置hexo文件夹下的`_config.yml`文件，最底下：

    deploy: 
      type: github
      repository: https://github.com/zzwei-/zzwei-.github.io/  //改成自己的用户名
      branch: master

9.执行`hexo g` ,`hexo d`,有可能报错 `Error:Deployer not found:github`，则执行:  

`npm install hexo-deployer-git --save`  

修改`_config.yml`文件：  

    deploy:
      type: git

再次执行上面的两条命令，在`https://yourname.github.io/`就可以看到自己的博客了。

本篇博客的内容来自[http://www.cnblogs.com/liulangmao/p/4323064.html](http://www.cnblogs.com/liulangmao/p/4323064.html)，详细内容请参考此博客。  
更多hexo的使用教程参考hexo官网：[https://hexo.io/](https://hexo.io/)