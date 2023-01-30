<template>
  <main>
    <Game
      v-on:click="$refs.input.focus()"
      :isFocused="isInputFocused"
    >
      <Caret
        v-if="isInputFocused"
        :currentWordEl="currentWordEl"
        :currentLetterIndex="currentInput.length"
      />
      <Words>
        <Word 
          v-for="word, i in words"
          :isCurrent="i === currentWordIndex"
          :currentInput="i === currentWordIndex ? currentInput : ''"
          :isComposing="i === currentWordIndex ? isComposing : false"
          :userInput="userWords[i]"
          :word="word"
          :key="i"
          ref="word"
        />
      </Words>
      <input 
        type="text"
        class="game-inputter"
        ref="input"
        v-on:focus="isInputFocused = true"
        v-on:blur="isInputFocused = false"
        v-model.trim="currentInput"
        v-on:keydown.space.prevent="onSpaceDown"
        v-on:keydown.backspace="onBackspace"
        v-on:keyup.space.prevent
        v-on:compositionstart="onCompositionStart"
        v-on:compositionupdate="onCompositionUpdate"
        v-on:compositionend="onCompositionEnd"
      >  
    </Game>

    <!-- <div class="debug">
      <ul>
        <li>
          currentWordIndex: {{currentWordIndex}}
        </li>
        <li>
          currentInput.length: {{currentInput.length}}
        </li>
        <li>
          currentWordEl: {{currentWordEl}}
        </li>
        <li>
          currentInput: {{currentInput}}
        </li>
        <li>
          currentWord: {{currentWord}}
        </li>
        <li>
          {{userWords}}
        </li>
      </ul>
    </div> -->
  </main>
</template>

<script>
import PhraseGen from 'korean-random-words';
import { ref } from 'vue'

import Game from '@/components/Game.vue'
import Caret from '@/components/Caret.vue'
import Words from '@/components/Words.vue'
import Word from '@/components/Word.vue'

export default {
  setup() {
    const phraseGen = new PhraseGen()
    const wordsArr = []
    for (let i=0; i < 5; i++) {
      phraseGen.generatePhrase().split('-').forEach(w=> {
        wordsArr.push(w)
      })
    }
    const words = ref(wordsArr)
    const userWords = ref([])
    const currentWordIndex = ref(0)
    const currentInput = ref("")
    const currentWordEl = ref(null)
    const isComposing = ref(false)
    const isInputFocused = ref(false)
    const lengthBeforeComposition = ref(0)
    return {
      words,
      userWords,
      currentWordIndex,
      currentInput,
      currentWordEl,
      isComposing,
      lengthBeforeComposition,
      isInputFocused
    }
  },
  mounted() {
    this.currentWordEl = this.$refs.word[0].$el
    document.addEventListener('keydown', this.focusGameInputter, false)
  },
  beforeUnmount() {
    document.removeEventListener('keydown', this.focusGameInputter, false)
  },
  watch: {
    currentWordIndex(n) {
      this.currentWordEl = this.$refs.word[n]?.$el || null
    },
    currentInput(n) {
      this.userWords.splice(this.currentWordIndex, 1, n)
    
      
    }
  },
  methods: {
    onBackspace(e) {
      if (this.isComposing) return
      if (this.currentInput.length < 1) {
        console.log("Backspace allowed and processed with prevent default")
        e.preventDefault()
        this.userWords.pop()
        this.currentWordIndex = Math.max(this.currentWordIndex - 1, 0)
        this.currentInput = this.userWords[this.currentWordIndex] || ""
      }
    },
    onCompositionStart(e) {
      console.log('onCompositionStart', e)
      this.lengthBeforeComposition = this.currentInput.length
      this.isComposing = true
      // this.currentInput = this.currentInput.substring(0, this.lengthBeforeComposition) + e.data
    },
    onCompositionEnd(e) {
      console.log('onCompositionEnd', e)
      this.isComposing = false
      if (e.data.slice(-1).trim().length < 1) {
        this.onInputSpace()
      }
    },
    onCompositionUpdate(e) {
      console.log('onCompositionUpdate', e)
      this.currentInput = this.currentInput.substring(0, this.lengthBeforeComposition) + e.data
    },
    onSpaceDown(e) {
      if (this.isComposing) {
        return
      }
      this.onInputSpace(e)
    },
    onSpaceUp(e) {
      if (this.isComposing) {
        return
      }
      this.onInputSpace(e)
    },
    onInputSpace() {
      if (this.currentInput.length < this.currentWord.length) {
        return
      }
      this.userWords.push("")
      this.currentWordIndex++
      this.currentInput = ""
    },
    focusGameInputter() {
      this.$refs.input.focus()
    }
  },
  computed: {
    currentWord() {
      return this.words[this.currentWordIndex]
    },
    currentLine() {
      const wordEl = this.currentWordEl
      if (wordEl) {
        const wordComputedStyle = getComputedStyle(wordEl)
        const wordGapY = Number(wordComputedStyle.getPropertyValue('margin-top').replace('px', '')) 

        const wordElRect = wordEl.getBoundingClientRect()
        const wordElPosYFromGameBox = wordEl.offsetTop
        const wordHeight = wordElRect.height
        const wordOuterHeight = Math.round(wordHeight + (wordGapY * 2))
        console.log({wordElPosYFromGameBox, wordHeight, wordOuterHeight, wordGapY})
        return Math.round(wordElPosYFromGameBox / wordOuterHeight) + 1
      } else {
        return 1
      }
    },
  },
  components: {
    Game,
    Caret,
    Words,
    Word,
  }
}
</script>

<style scoped>
main {
  width: 100%;
  height: 100%;
}
.debug {
  position: fixed;
  bottom: 0;
  left: 0;
}

.game-inputter {
  position: fixed;
  bottom: 0;
  left: 0;
  /* opacity: 0; */
}
</style>