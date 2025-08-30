### 介绍
HOJ(⚡🔥Hcode Online Judge(HOJ)🔥⚡：基于SpringCloud与Vue前后端分离，分布式架构的在线测评平台OJ (An open source online judge system base on SpringBoot, Springcloud Alibaba and Vue.js !)) 来自why的二开版本，使用请您注明仓库地址，谢谢

### 如何安装？
- 请您先进行HOJ的部署，https://gitee.com/himitzh0730/hoj-deploy
- 部署完成后，请您 ```git clone https://gitee.com/whysblog/hoj-why.git /root/hoj-vue```
###如何使用
``` cd /root/hoj-vue 进入仓库```
#### 第一次使用 安装依赖
```
npm install
```

### 编译
```
npm run build
```
等待编译完成后，请您将dist文件放在你喜欢的地方，比如 /hoj/www/html/dist，然后修改 docker-compose.yml，在 hoj-frontend 模块中的 volumes 中增加一行 - /hoj/www/html/dist:/usr/share/nginx/html （冒号前面的请修改为实际的路径），然后 docker-compose up -d 即可。
### 我们更改的内容？
1.在线IDE默认使用C++
2.题目AC弹出礼花
3.公开课程添加榜单
4.私有比赛/课程简介也要先输入密码
5.可赛后提交并有赛后榜单添加（vp）标签
6.赛中/后榜单独立
7.修复只有二开版本问题（讨论不显示）
8.添加后台图形化用户添加
