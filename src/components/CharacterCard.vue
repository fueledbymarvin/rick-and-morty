<template>
  <div class="card">
    <img :src="character.image" />
    <div class="info">
      <div class="name">{{ character.name }}</div>
      <div class="location">{{ character.location.name }}</div>
    </div>
  </div>
</template>

<script setup="props">
import { defineProps, toRefs } from 'vue'

const props = defineProps({
  character: Object,
})
const { character } = toRefs(props)
</script>

<style lang="scss" scoped>
.card {
  display: flex;
  border-radius: 8px;
  overflow: hidden;
  box-shadow:
    0px 2px 4px rgba(58, 92, 144, 0.14),
    0px 3px 4px rgba(58, 92, 144, 0.12),
    0px 1px 5px rgba(58, 92, 144, 0.2);
}

img {
  flex-shrink: 0;
  width: 96px;
  height: 96px;
}

.info {
  min-width: 0;
  padding: 16px;
}

.name {
  font-size: 20px;
  font-weight: bold;
}

.location {
  color: #878D96;
  margin-top: 8px;
}

.name, .location {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.buttons {
  margin-top: 24px;
  display: flex;
  justify-content: space-between;

  button {
    background: #FFF;
    border: 1px solid #343A3F;
    box-shadow:
      0px 2px 4px rgba(58, 92, 144, 0.14),
      0px 3px 4px rgba(58, 92, 144, 0.12),
      0px 1px 5px rgba(58, 92, 144, 0.2);
    border-radius: 8px;
    padding: 8px 16px;
    text-transform: uppercase;
    color: #343A3F;
    font-size: 12px;
    font-weight: bold;
    line-height: 1;
  }
}
</style>
