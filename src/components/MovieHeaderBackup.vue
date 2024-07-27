<template>
  <div>
    <header class="header">
        <div class="logo">FLOX</div>
        <nav class="nav">
          <router-link to="/">电影列表</router-link>
          <router-link to="/cinema">影院列表</router-link>
          <router-link to="/user">个人中心</router-link>
          <router-link to="/order">我的订单</router-link>
        </nav>
      </header>
      <div class="search-bar">
        <input type="text" placeholder="Search" @keydown="SearchButtonDown"/>
      </div>
  </div>
</template>

<script setup>
  function SearchButtonDown(event) {
    if (event.code == "Enter") {
      
    }
  }
</script>