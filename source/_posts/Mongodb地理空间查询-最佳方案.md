---
title: Mongodb地理空间查询-最佳方案
date: 2017-06-4 12:45:38
categories: 后端开发
author: 阿文
tags:
  - Mongodb
  - 地理空间
---







> MongoDB
是一个基于分布式文件存储的数据库。由[C++](http://baike.baidu.com/view/824.htm)语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。

## 前言
在移动开发中，经常会用到定位的功能，例如美团、饿了么、猫眼电影等应用，都使用了定位经纬度，可以查询用户位置附近的一些服务、商家等信息。

本篇文章将会讲述如何利用Mongodb进行数据存储、定位查询。

本方案为最佳实践，实现功能如下：
* 带经纬度数据的存储
* 根据用户经纬度，查询由近到远的数据（分页）
* 附带距离数值（数据库层级的计算）
* 可设置最大距离

一句话形容就是 ： 存储带经纬度的数据，并且根据用户的不同位置，返回可分组、带距离的数据列表

<!-- more -->

注意，Mongodb版本在2.4以上

## 分析
举个例子，我们需要做一个应用，管理员可以把商家信息发布到应用上，用户打开应用可以看到距离自己由近到远的商品，并且可以看到距离多少。

Mongodb有一种索引--地理空间索引，利用它可以进行经纬度的计算，下面继续介绍如何使用该功能。

## 实现
下面以Nodejs+mongoose为例

1. 创建Schema：
```
const mongoose = require( 'mongoose' );
let VenuesSchema = new mongoose.Schema( {
    name: String,
    address: String,
    location: {
        type: [ Number ],
        index: {
            type: '2dsphere',
            sparse: true
        }
    }
}, {
    collection: 'Venues'
} )
```

2. 创建Model
```
let VenuesModel = mongoose.model(‘Venues’, VenuesSchema)
```

3. 插入数据
```
按照以下数据格式往数据库插入数据：
{
    "name": "音乐派KTV(银泰城)",
    "address": "武侯区高新区天府四街益州大道口银泰城4F"
    "location" : [
        104.05801,
        30.5411
    ],
}
```

4. 查看用户附近的数据
```
  let matchQuery = {
    “name” : /音乐/ig
  }   // 额外筛选,此处查询名字包含音乐的数据
  let venuesCount = await VenuesModel.count().exec()

  VenuesModel.aggregate( [
    {
      $geoNear: {
         near: { type: 'Point', coordinates: [
            104.05801 ,  //用户经度
            30.5411   //用户纬度
         ] },
         distanceField: 'distance',  // 距离数值字段名
         spherical: true,
         maxDistance: 1000    // (Int类型的最大距离--米，例如500，1000),        
         query: matchQuery,
         limit: venuesCount
      }
    },
    {
      $match: matchQuery
    },
    {
      $count: 'venuesCount'
    },  //此处添加了此处，则返回数据量，如需返回具体数据列表，删除此处
    {
      $skip: 30  
    }, // 分页，跳过数据数量
    {
      $limit: 30
    } // 分页，返回的数据数量
 ] )
```

## 总结
本次主要分享位置索引和aggregate的用法，十分实用的一个功能。

* 返回的数据默认就是近到远的数据
* 不要忘了数据库字段location的索引配置
* 距离信息会附带在返回的信息列表中，会有一个distance字段（我们定义的，可以修改）
