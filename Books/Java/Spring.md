# 概述与安装

## 概述

Spring框架是轻量级的JavaEE框架。

**核心部分：IOC、AOP**

- IOC
  - 控制反转，把创建对象的过程交给Spring进行管理
- AOP
  - 面向切面，不修改源代码进行功能增强

![](vx_images/78920817236840.png =520x)

### IOC容器

**IOC**

- 控制反转，把对象的创建和对象之间的调用过程，交给Spring进行管理。
- 使用IOC目的：降低耦合度

#### IOC底层原理

**未使用IOC时**

![](vx_images/458572217250474.png =717x)

![](vx_images/215182317248078.png =725x)

**IOC过程  xml解析、工厂模式、反射**

- IOC思想基于IOC容器完成，IOC容器底层就是对象工厂

![](vx_images/60922917245580.png =724x)

#### IOC接口(BeanFactory)

- BeanFactory接口
   - IOC容器基本实现，是Spring内部的使用接口，不提供给开发人员使用。
   - 加载配置文件时，不会创建对象，而是在获取对象（使用）时才创建对象。
- ApplicationContext接口
   - BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用。
   - 



#### IOC操作Bean管理（基于xml）
#### IOC操作Bean管理（基于注解）

## 安装

**下载**

https://spring.io/projects/spring-framework#learn

https://repo.spring.io/ui/native/release/org/springframework/spring

https://repo.spring.io/artifactory/release/org/springframework/spring/5.2.6.RELEASE/spring-5.2.6.RELEASE-dist.zip

1. 使用GA稳定版本


 