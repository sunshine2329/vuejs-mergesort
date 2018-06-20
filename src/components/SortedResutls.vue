<template>
<div id="sorted-results">
  <div style="position:absolute;top:20px;height:80px;width:100%">
    <p>You are finished!</p>
    <p>Here are your results:</p>
  </div>
  <div style="overflow: hidden;">
    <p>
      <ol style="overflow-y: auto; margin-top: 80px; height: calc(100vh - 210px)">
        <li v-for="(phrase, index) in results" v-bind:key="`phrase-${index}`">{{ phrase }}</li>
      </ol>
    </p>
  </div>
  <div style="position:absolute;bottom:20px;height:50px;">
    <button v-on:click.prevent="onBack">Back</button>
    <button v-on:click.prevent="onRestart">Restart</button>
  </div>
</div>
</template>

<script>
export default {
  props: {
    results: {
      type: Array,
      required: true
    }
  },
  methods: {
    onRestart: function () {
      this.$emit('onStart', true)
    },
    onBack: function () {
      this.$emit('initialize')
    }
  }
}
</script>

<style scoped>
#sorted-results {
  overflow-y: auto;
  justify-content: center;
  display: flex;
}

#sorted-results ol {
  text-align: left;
}

#sorted-results li {
  display: list-item;
}
</style>
