<template>
  <button>
    <slot></slot>
  </button>
</template>

<style lang="scss" scoped>
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
  cursor: pointer;
}
</style>
