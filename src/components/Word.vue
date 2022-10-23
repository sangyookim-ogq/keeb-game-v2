<template>
<div class="word-outer">
  <div 
    :class="[
      'word',
      {
        'word-is-current': isCurrent
      }
    ]"
  >
    <Letter
      v-for="char, i in word"
      :letter="char"
      :userLetter="userInput[i]"
      :isComposing="i === (userInput.length - 1) ? isComposing : false"
      :key="i"
    />
  </div>
  <div>
    <!-- {{currentInput}} -->
    <!-- {{userInput}} -->
  </div>
</div>
</template>

<script>
import Letter from './Letter.vue'
export default {
  props: {
    isCurrent: {
      type: Boolean,
      default: false
    },
    word: {
      type: String
    },
    currentInput: {
      type: String,
      default: ""
    },
    userInput: {
      type: String,
      default: ""
    },
    isComposing: {
      type: Boolean,
      default: false
    },
  },
  components: {
    Letter
  }
}
</script>

<style lang="scss">
.word-outer {
  margin: 0 4px;
}
.word {
  white-space: nowrap;
  font-size: 24px;
  line-height: 1.45;
  // opacity: 0.6;
  font-weight: 400;
}

.word-is-current {
  // color: yellow;
  opacity: 1;
}
</style>