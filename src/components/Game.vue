<template>
<div class="game-outer">
  <div class="game">
    <div class="game-inner">
      <slot />
    </div>
  </div>
</div>
</template>

<style lang="scss" scoped>
.game-outer {
  display: flex;
}

.game {
  position: relative;
  width: 100%;
  max-width: 740px;
  margin: 0 auto;
}

.game-inner {
  position: relative;
  margin-left: -8px;
  margin-right: -8px;
  padding: 0 8px;
}
</style>