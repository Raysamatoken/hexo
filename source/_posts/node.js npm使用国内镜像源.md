---
title: node.js npm使用国内镜像源
date: 2022-01-19 10:19:05
tags:
---
### (1) 使用 npm
```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
npm i
```
### (2) 使用 yarn
```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
npm i -g yarn
yarn
```