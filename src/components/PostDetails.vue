<template>
  <div class="text-justify py-2 m-0">
      <p
        class="m-0"
        v-for="value, key in data">
        <strong>{{ key }}</strong>: {{ value }}
      </p>
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
