---
layout:     post
date:       2018-12-29
title:      如何根据「平级结构」的集合构建「树形结构」
cover:  https://siyuanlau.github.io/img/random/material-7.png
categories: 后台开发
tags:
    - 菜单设计
    - 数据库
---
在数据库设计的一些场景里面，我们经常把一些树形结构的数据设计成平级结构。比如，菜单是一个典型的树形结构。每个菜单有一个父菜单，同时可以有多个子菜单。设计菜单数据库的时候，我们会给每个菜单定义一个parentId的字段，用来指定它的父菜单。一个parentId字段就可以表达菜单的父子关系了。因为数据库都是行列式的存储，这样的数据就相当于平级结构。而通过parentId这样的设计，我们就可以使用平级结构的数据来存储树状结构的数据。这种设计很常用，比如一个电商里面的分类表，一个OA系统里面的组织结构表，都是类似的树形结构。

存储数据的时候，我们会把树形结构的数据转化成平级结构。反过来，当我们查询数据的时候，通常又要把平级结构转回树状结构。比如很多前端UI框架里面，构建一个树组件的时候，都需要提供一个树形结构的数据。

如何高效的把平级结构数据转化成树形结构呢？

<!--more-->

## 背景
在数据库设计的一些场景里面，我们经常把一些树形结构的数据设计成平级结构。比如，菜单是一个典型的树形结构。每个菜单有一个父菜单，同时可以有多个子菜单。设计菜单数据库的时候，我们会给每个菜单定义一个parentId的字段，用来指定它的父菜单。一个parentId字段就可以表达菜单的父子关系了。因为数据库都是行列式的存储，这样的数据就相当于平级结构。而通过parentId这样的设计，我们就可以使用平级结构的数据来存储树状结构的数据。这种设计很常用，比如一个电商里面的分类表，一个OA系统里面的组织结构表，都是类似的树形结构。

存储数据的时候，我们会把树形结构的数据转化成平级结构。反过来，当我们查询数据的时候，通常又要把平级结构转回树状结构。比如很多前端UI框架里面，构建一个树组件的时候，都需要提供一个树形结构的数据：
```
{
    id: 1,
    name: '根菜单',
    children: [
        {id: 11, name: '子菜单一'},
        {id: 12, name: '子菜单二'},
    ]
}
```

而数据库里面的数据是这样的：

id |name |parentId
:---:|:---:|:---:
1  | 根菜单 | null
11 | 子菜单一 | 1
12 | 子菜单二 | 1

这个怎么做呢？

## 解决方式
菜单类：
```
class Menu {
    private Integer id;
    private String name;
    private Integer parentId;
    private List<Menu> children;

    // getter and setter
}
```

构建步骤：
```
/** 时间复杂度：O(n) */
Menu buildTree(List<Menu> menus) {
    Menu root = null;
    Map<Integer, Menu> lookup = new HashMap<>();

    // 查找表，可以方便地根据id查询对应元素
    for (Menu m : menus) {
        lookup.put(m.getId(), m);
    }

    // 将每个节点添加到它对应的父节点的children
    for (Menu m : menus) {
        Menu parent = map.get(m.getParentId());

        if (parent != null) {
            parent.getChildren().add(m);
        }
    }

    // 返回根节点。通过它的childrend就可以遍历到所有的子节点。
    for (Menu m : menus) {
        if (m.getParentId == null) {
            root = m;
            break;
        }
    }
    
    return root;
}
```
