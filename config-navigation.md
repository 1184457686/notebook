---
title: react-navigation 的配置
date: 2020-05-03 16:56:53
tags: React-Native
---

>一定要查看官方的英文文档，其他中文文档有滞后，版本过低

### 1. 安装基本模块

```shell
npm i @react-navigation/native
```

### 2. 安装所需要的依赖项库

```shell
npm i react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

### 3. 安装三种导航模式库

```shell
npm i @react-navigation/stack @react-navigation/bottom-tabs @react-navigation/drawer
```

* StackNavigator: 栈导航
* TabNavigator: 标签导航
* DrawerNavigator: 抽屉导航