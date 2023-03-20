# MVVM框架

## 什么是MVVM框架

MVVM框架指的是 Model（模型）、View（视图）、View Model（视图模型）的架构模式。

View改变流程：用户输入 --> View --> ViewModel --> Model

Model改变流程: Ajax请求--> Model --> ViewModel --> View

## 什么是MVC框架

MVC框架指的是Model、View、Controller的架构模式。

View流程：用户输入 --> View  ---> Controller ---> Model --->View

Model改变流程: Ajax请求--> Model -->  Controller ---> View

## MVVM对MVC框架的优化

- 在ViewModel中通过数据绑定和DOM事件监听，将View和Model层进行了解耦。利于两者的复用。

- 解决了MVC中大量DOM操作提升渲染性能。