<template>
  <div class="item about-item">
    <div class="item-content drop-shadow about card about-card active m-2 p-5">
      <h4>{{ data.title }}</h4>
      <p class="mb-0">{{ data.body }}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: "about",
    props: ['data'],
  }
</script>

<style lang="scss">
@import "../variables";

.about-card {
  max-width: 700px;
}
.image-container {
  overflow: hidden;
}

</style>
