<template>
<div id="start-sort">
  <p>Instructions:</p>
  <p>Choose which one you fee is the better of the two options.</p>
  <button v-on:click.prevent="onStart" style="margin-top: 30px">Start</button>
</div>
</template>

<script>
export default {
  props: {
    start: {
      type: Boolean
    }
  },
  data () {
    return {}
  },
  methods: {
    onStart: function () {
      this.$emit('onStart')
    }
  }
}
</script>

<style scoped>
#start-sort {
  height: 100%;
  padding-top: 40px;
}
</style>
