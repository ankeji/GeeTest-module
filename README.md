<!--
 * @Descripttion: 
 * @version: 
 * @Author: ankeji
 * @Date: 2020-10-14 14:37:58
 * @LastEditors: ankeji
 * @LastEditTime: 2020-10-14 14:49:15
-->

# GeeTest-module

极验验证的封装
极验组件会暴露出一个geetest方法和返回值，可以使用this.$bus.$on("geetest", data)监听

## 全局引入gt.js

全局注册GeeTest.vue组件

## 需要调用的地方直接使用bus传值进行触发

  this.$bus.$emit('login_geetest')

## 同时在调用的组件里面created生命周期进行监听

  this.$bus.$on("geetest", data); //极验返回的值

## 切记在本组件beforeDestroy生命收起里面进行取消监听（否者会触发多次bus）

  this.$bus.$off("geetest");
