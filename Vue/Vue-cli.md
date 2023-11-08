# Vue-cli

## 什么是Vue-cli

![image-20231106143431233](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311061434490.png)

## 安装

```shell
npm install vue-cli -g # -g全局安装
```

查看可以基于哪些模板创建vue程序，通常我们选择webpack

```shell
vue list
```

## 使用

在想要建立项目的文件夹下

```shell
# 这里的myvue是项目名称，可以根据自己的需求起名
vue init webpack myvue
```

会有一系列选项，按需选取即可

![image-20231106144232305](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311061442165.png)

之后使用

```shell
npm install
```

安装项目所需依赖，等到安装完成后

```shell
npm run dev
```

即可运行

## 各目录含义

详见

> https://blog.csdn.net/dxnn520/article/details/123712506

![img](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311061445396.png)