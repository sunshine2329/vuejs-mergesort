<style scoped>
#choose-phrase button {
  width: 100%;
}
</style>

<template>
<div id="choose-phrase">
  <div style="text-align: left">
    <button v-on:click.prevent="onBack" style="width: auto;">Back</button>
  </div>
  <p>Instructions:</p>
  <p>Choose which one you fee is the better of the two options.</p>
  <p style="margin-top: 30px">
    <button v-on:click.prevent="sortPhrases(true)">{{ phrase1 }}</button>
  </p>
  <p>
    <button v-on:click.prevent="sortPhrases(false)">{{ phrase2 }}</button>
  </p>
</div>
</template>

<script>
export default {
  props: {
    phrases: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      leftPhrasesSet: [],
      sortedPhrasesSet: [],
      history: [],
      phraseSet1: [],
      phraseSet2: [],
      sortedPhrases: [],
      phrase1: '',
      phrase2: ''
    }
  },
  created () {
    console.log('created')
    this.initialize()
    this.getNextPhrases()
  },
  methods: {
    initialize: function () {
      for (var i = 0; i < this.phrases.length; i++) {
        this.leftPhrasesSet.push([this.phrases[i]])
      }
      this.sortedPhrasesSet = []
      this.history = []
      this.phraseSet1 = []
      this.phraseSet2 = []
      this.sortedPhrases = []
      this.phrase1 = ''
      this.phrase2 = ''
    },
    getNextPhrasesSets: function () {
      var len = this.leftPhrasesSet.length
      if (len > 1) {
        if (this.sortedPhrases.length > 0) {
          this.sortedPhrasesSet.push(this.sortedPhrases)
          this.sortedPhrases = []
        }
        this.phraseSet1 = this.leftPhrasesSet[0]
        this.phraseSet2 = this.leftPhrasesSet[1]
        if (len === 2) {
          this.leftPhrasesSet = []
        } else {
          this.leftPhrasesSet = this.leftPhrasesSet.slice(2)
        }
      } else {
        if (len === 1) {
          this.sortedPhrases = this.sortedPhrases.concat(this.leftPhrasesSet[0])
        }
        if (this.sortedPhrases.length === 0 || this.sortedPhrases.length === this.phrases.length) {
          this.$emit('onEnd', this.sortedPhrases)
        } else {
          if (this.sortedPhrases.length > 0) {
            this.sortedPhrasesSet.push(this.sortedPhrases)
          }
          this.sortedPhrases = []

          this.leftPhrasesSet = this.sortedPhrasesSet
          this.sortedPhrasesSet = []
          this.getNextPhrasesSets()
        }
      }
    },
    getNextPhrases: function () {
      var len1 = this.phraseSet1.length
      var len2 = this.phraseSet2.length
      if (len1 === 0 || len2 === 0) {
        if (len1 === 0) {
          this.sortedPhrases = this.sortedPhrases.concat(this.phraseSet2)
        } else if (len2 === 0) {
          this.sortedPhrases = this.sortedPhrases.concat(this.phraseSet1)
        }
        this.getNextPhrasesSets()
      }

      if (this.phraseSet1.length > 0 && this.phraseSet2.length > 0) {
        this.phrase1 = this.phraseSet1[0]
        this.phrase2 = this.phraseSet2[0]
      }
      this.history.push({
        leftPhrasesSet: this.leftPhrasesSet,
        sortedPhrasesSet: this.sortedPhrasesSet,
        phraseSet1: this.phraseSet1,
        phraseSet2: this.phraseSet2,
        sortedPhrases: this.sortedPhrases,
        phrase1: this.phrase1,
        phrase2: this.phrase2
      })
    },
    sortPhrases: function (isFirst) {
      if (isFirst) {
        this.sortedPhrases.push(this.phrase1)
        this.phraseSet1 = this.phraseSet1.slice(1)
      } else {
        this.sortedPhrases.push(this.phrase2)
        this.phraseSet2 = this.phraseSet2.slice(1)
      }
      this.getNextPhrases()
    },
    onBack: function () {
      if (this.history.length === 0) {
        this.$emit('initialize')
      } else {
        let temp = this.history.pop()
        this.leftPhrasesSet = temp.leftPhrasesSet
        this.sortedPhrasesSet = temp.sortedPhrasesSet
        this.phraseSet1 = temp.phraseSet1
        this.phraseSet2 = temp.phraseSet2
        this.sortedPhrases = temp.sortedPhrases
        this.phrase1 = temp.phrase1
        this.phrase2 = temp.phrase2
      }
    }
  }
}
</script>
