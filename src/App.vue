<template>
  <div class="page-content">
    <Search />
  </div>
</template>

<script setup>
// show page number, total results
// consistent border-radius and spacing
// use common fonts
// show ellipsis? or should we just see what they do
// 15 min setup
// 30 min unstyled search
// 30 min style search
import Search from './components/Search.vue'
</script>

<style lang="scss">
@import './reset.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 16px;
  color: #000;
  display: flex;
  justify-content: center;
}
</style>

<style lang="scss" scoped>
.page-content {
  padding: 16px;
  flex: 0 1 400px;
  max-width: 100%;
}
</style>
