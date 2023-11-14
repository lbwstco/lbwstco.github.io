---
layout: post
title: 导入HoloEverywhere至Android Studio
description: 经过大半天的折腾，总算在Android Studio环境下将HoloEverywhere作为library引入到自己的项目中了，以下是操作步骤。类似的，引用其他项目也可以使用如下的方法。
category: android
keywords: Android Studio, HoloEverywhere 
--- 


####操作步骤####


1. 从https://github.com/Prototik/HoloEverywhere下载最新版本，这里也可以使用git clone;
2. 新建module，选择Android Library，命名为holoeverywhere-library(可以自己取其他的名称);
3. 删除新建的module下src/main目录下所有内容;
4. 将刚刚下载下来的HoloEverywhere项目解压，拷贝library目录下的src、res文件夹及AndroidManifest.xml至holoeverywhere-library的src/main目录下;
5. 修改src文件夹的名称为Java;
6. 拷贝library/libs目录中的nineoldandroids.jar以及support-v4.jar至项目更目录的libs文件夹下
7. 修改holoeverywhere-library中的build-gradle文件中的dependencies语句块，添加:

		compile files('../libs/nineoldandroids.jar')
		compile files('../libs/support-v4.jar')

8. 添加项目对holoeverywhere-library的依赖，Rebuild Project


