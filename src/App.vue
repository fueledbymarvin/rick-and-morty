<template>
  <div class="page-content">
    <template v-if="characterId">
      <Button @click="characterId = null">Return to search</Button>
      <div class="character">
        <Character :id="characterId" @character="(id) => characterId = id" />
      </div>
    </template>
    <div v-show="!characterId">
      <div class="title">Character Explorer</div>
      <div class="search">
        <Search @character="(id) => characterId = id" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Search from './components/Search.vue'
import Character from './components/Character.vue'
import Button from './components/Button.vue'

const characterId = ref(null)
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

.title {
  font-weight: bold;
  font-size: 24px;
}

.character {
  margin-top: 32px;
}

.search {
  margin-top: 16px;
}
</style>
