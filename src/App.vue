<template>
  <div id="app">
    <!-- 头部 -->
    <app-header></app-header>
    <!-- 导航 -->
    <app-nav></app-nav>
    <!-- 内容 -->
    <router-view></router-view>
  </div>
</template>

<script>
import Header from "./components/header/Header";
import Nav from "./components/nav/Nav";

export default {
  name: "App",
  components: {
    appHeader: Header,
    appNav: Nav
  }
};
</script>

<style>
</style>
