---
title: SpringBoot 的扩展接口汇总大全（都给你们整理好啦！）
shortTitle: SpringBoot 扩展汇总
categories:
  - [SpringBoot]
tags:
  - 扩展
  - SpringBoot
  - 汇总
description: 
date: 2024-03-11
keywords: 'SpringBoot,spring,扩展,汇总,总结'
cover: 
abbrlink: 112233
---

# 接口

## MethodInterceptor

这是一个接口，用于实现方法拦截器。方法拦截器可以在目标方法执行前、执行后或异常抛出时拦截方法调用，并执行相应的逻辑。




# 抽象类

## StaticMethodMatcherPointcut

这是一个 Spring 框架中的类，用于定义静态方法匹配切点。它提供了一种方式来匹配目标对象中的方法，以确定是否应该应用切面的通知逻辑。

## AbstractBeanFactoryPointcutAdvisor

这是一个抽象类，用于实现切面通知者。它结合了切点和通知，提供了一种将切面应用于 Spring Bean 的方式。


# 执行顺序

AbstractBeanFactoryPointcutAdvisor
StaticMethodMatcherPointcut

MethodInterceptor


调用接口时：
MethodInterceptor
