<template>

  <span 
    :class="[
      'letter',
      {
        'letter-incorrect': isIncorrect,
        'letter-correct': isCorrect,
        'letter-composing': isComposing
      }
    ]"
  >
    {{letter}}
  </span>

</template>

<script>
export default {
  props: {
    letter: {
      type: String,
      required: false
    },
    userLetter: {
      type: String,
      required: false
    },
    isComposing: {
      type: Boolean,
      required: false
    }
  },
  computed: {
    isInputted() {
      return this.userLetter != null
    },
    isCorrect() {
      return this.isInputted && this.letter === this.userLetter
    },
    isIncorrect() {
      return this.isInputted && !this.isComposing && this.letter !== this.userLetter 
    }
  },
  setup() {
    
  },
}
</script>

<style lang="scss" scoped>
.letter {
  position: relative;
  display: inline-block;
}
.letter-correct {
  font-weight: 500;
}
.letter-incorrect {
  color: #FF0000;
}
.letter-composing:after {
  position: absolute;
  content: "";
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background-color: black;
}
</style>