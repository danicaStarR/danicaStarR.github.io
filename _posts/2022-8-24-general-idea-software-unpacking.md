---
layout: post
title:  "软件脱壳通用思路"
date:   2022-8-24 20:32:42 +0800
categories: Page
tags: Jekyll
img: https://s2.loli.net/2022/08/24/TqIMuZ4k1Ki6mex.jpg
---

# Android逆向之脱壳
脱壳一般指去除加固包。

已知脱壳有三种手段：
1. Xposed：例反射大师
2. VM：例blackdex
3. Frida
![threeSoftWare.jpg](https://s2.loli.net/2022/08/24/ROLyIMNzuVc62PT.jpg)

每个手段都有不同的用法。

## 一般步骤
1. 去除签名验证（大部分加壳都有验证，推荐用np的modex3.0，推荐选精简包）
2. 脱壳  
   - 反射大师：需要xp框架。点击反射大师，选择应用，打开应用（注意要有悬浮窗，没有就多试几次，推荐用旧版）。打开悬浮窗，长按提取Dex（单击只能转一个dex），然后根据路径找就行
   ![fs.jpg](https://s2.loli.net/2022/08/24/jiuPYecJ942QrC8.jpg)
   - BlackDex：注意分32位和64位。建议关闭深度脱壳
   - Frida或Armpro其他
3. 修复Dex文件：MT或NP
4. 获取App入口：改android:name。入口名一般在壳外classes.dex就有，如果为空，那么则软件没有入口。
5. 移dex：
   - 没有签名效验或非modex：
   1. 删除原来classes.dex
   2. 把修复好的dex移过来
   - modex直接把弄好的dex放到assets/App_Dex动态加载
   > 要注意把assets/App_So里面的东西放到lib下（看情况）
6. 签名安装测试
## 闪退的几种可能
1. 签名效验
2. np选的精简包或原包问题
3. app入口错误
4. dex代码抽取不完整
