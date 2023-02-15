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
  color: #949494;
}
.letter-correct {
  font-weight: 500;
  color: #2A2A2A;
}
.letter-incorrect {
  color: #DE212D;
}
.letter-composing:after {
  position: absolute;
  content: "";
  top: 100%;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  margin-top: -2px;
  background-color: #949494;
}
</style>