# alinesno-infra-plugins

## 概述

> 插件机制是应用在较为严格的工程规范之下，在不熟悉情况下，请勿根据个人喜好进行插件改造。

代码插件库，用于集成多种插件插件，集成到工程生成器中，形成统一的管理风格，类似于helm或者wordpress的插件原理类似。

使用类似于spring-boot-starter方式加载工程，插件机制跟starter机制有一定的区别，这里是针插件源码更新到工程，
而不是jar包依赖的形式，主要是方便进一步调试处理，用于针对于业务服务开发的工具，另一方面，这里的插件理解成工程一部分，这个统称为plugin。

## 插件场景

插件处理的场景如下，处理的方式主要考虑因素如下：

- 插件为工程一部分，而非公共部分；
- [重点]插件的维护职责下放到项目组，明确平台与项目组职责划分；
- 工程的个性化配置和自主性，同样为了明确职责划分；
- 插件在有相对稳定或者公共部分，依赖团队抽取成jar/npm等;

注意：插件机制参考的是wordpress机制进行管理，而非starter机制。

## 插件规范

为了更便于插件的管理和维护.功能插件[function]：功能插件即带有对应的功能处理，数据库处理，页面处理等，形成一个功能原子，需分带有api/domain/ui插件

### 目录规范

目录规范是针对于插件与工程的目录规范，插件统一放到工程的`alinesno-cloud-plugins`目录下面。

### 名称规范

这里定义了基本的名称规范配置：

1. 插件目录结构：

当中定义官方插件前缀是`alinesno-infra`，团队的插件名称应该自定义前缀,格式如:xxxxx(团队标识)-plugin，用于做区分。
```sh
alinesno-plugin-[function]-enable   # 也可以使用springboot自动加载机制
alinesno-plugin-[function]-controller
alinesno-plugin-[function]-service
alinesno-plugin-[function]-mapper
alinesno-plugin-[function]-entity
```

2. 坐标定义规范
```xml
<!-- 以api坐标为例定义 -->
<groupId>com.alinesno.infra.plugin</groupId>
<artifactId>alinesno-infra-plugin-[function]</artifactId>
```

3. 插件调试规范

- 插件的调试可使用`test`进行主类调试，打包过程不会含有`test`目录；
- 插件中不能包含`main`方法，统一使用`junit`调试；

4. 插件读取定义

插件读取定义是为了让生成器读取，类似于jenkins-plugins插件的读取机制，这里平台会读取公共的git文件配置，这里通过`yaml`定义，定义规划如下:

```yaml
plugins:
    alinesno-plugin-cms:
        type: function
        icon: #(图片base64)
        classes: 001 # 分类代码(从项目初始工程中获取)
        info:
            name: Flowable工作流插件  # 插件名称
            desc: 集成Flowable工作流模块，快速集成  # 插件描述
        team:
            name: AIP团队研发 # 团队信息
    alinesno-plugin-storage:
        type: function
        icon: #(图片base64)
        classes: 001 # 分类代码(从项目初始工程中获取)
        info:
            name: Flowable工作流插件  # 插件名称
            desc: 集成Flowable工作流模块，快速集成  # 插件描述
        team:
            name: AIP团队研发 # 团队信息
```

每个插件定义在基线根目录下面，过程会通过基线读取

## 其它

- 部分构建工具参考[dubbo](https://github.com/apache/dubbo)
