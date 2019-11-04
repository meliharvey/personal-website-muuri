<template>
  <div class="text-justify py-5 m-0">
      <p>{{ data.text }}</p>
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
