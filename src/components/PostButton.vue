<template>
  <div class=" text-center py-3">
    <b-button :href="data.link" variant="outline-dark">{{ data.text }}</b-button>
  </div>
</template>

<script>
  export default {
    props: ['data'],
  }
</script>

<style lang="scss">
@import "../variables";

</style>
