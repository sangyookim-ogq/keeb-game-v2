<template>
  <div 
    :class="[
      'caret'
    ]"
    :style="{
      transform: `translate3d(${caretPos.left + 'px'}, ${caretPos.top + 'px'}, 0)`
    }"
  >
  </div>
</template>

<script>
export default {
  data() {
    return {
      top: 0,
      left: 0
    }
  },
  props: {
    currentWordEl: [HTMLObjectElement, HTMLDivElement],
    // currentWordIndex: Number,
    currentLetterIndex: Number
  },
  watch: {
    currentLetterIndex() {
      this.setCaretPos()
    }
  },
  methods: {
    setCaretPos() {
      const wordEl = this.currentWordEl

      if (!wordEl) {
        this.top = 0
        this.left = 0
        return
      }

      const letterEls = Array.from( wordEl.querySelectorAll('.letter') )
      const letterLength = this.currentLetterIndex
      
      const validLetterEls = letterEls.slice(0, letterLength)
      const validLetterWidths = validLetterEls.map(el=> el.getBoundingClientRect().width || 0)
      const totalWidthOfLetters = validLetterWidths.reduce((a, b)=> a + b, 0)
      const left = totalWidthOfLetters + (wordEl.offsetLeft)
      const top = wordEl.offsetTop || 0

      this.top = Math.round(top)
      this.left = Math.round(left)
    }
  },
  computed: {
    // currentWordEl() {
    //   return Array.from(document.querySelectorAll('.word'))[this.currentWordIndex]
    // },
    caretPos() {
      const wordEl = this.currentWordEl

      if (!wordEl) {
        console.log("worldEl does not exist")
        return {
          top: 0,
          left: 0
        }
      }

      const letterEls = Array.from( wordEl.querySelectorAll('.letter') )
      const letterLength = this.currentLetterIndex
      
      const validLetterEls = letterEls.slice(0, letterLength)
      const validLetterWidths = validLetterEls.map(el=> el.getBoundingClientRect().width || 0)
      const totalWidthOfLetters = validLetterWidths.reduce((a, b)=> a + b, 0)
      const left = totalWidthOfLetters + (wordEl.offsetLeft)

      console.log('left', totalWidthOfLetters, wordEl.offsetLeft)
      const top = wordEl.offsetTop || 0

      return {
        left, top
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.caret {
  position: absolute;
  top: 1px;
  height: 29px;
  width: 3px;
  background-color: #1D1ADB;
  transition: 150ms transform, 10ms opacity;
  z-index: 99;
  // animation: blink 1s infinite alternate-reverse;
}

@keyframes blink {
  0% {
    opacity: 100%;
  }
  100% {
    opacity: 10%;
  }
}
</style>