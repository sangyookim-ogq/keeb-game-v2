<template>
  <div class="words">
    <slot />
  </div>
</template>

<style lang="scss" scoped>

.words {
  position: relative;
  display: inline-flex;
  align-items: center;
  // justify-content: center;
  flex-wrap: wrap;
  z-index: 1;
}

</style>