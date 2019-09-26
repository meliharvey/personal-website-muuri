<template>
  <div class="py-3">
    <div>
      <img :src="getImgUrl(data.link)" alt="" class="post-image">
    </div>
    <div class="py-1">
      <p class="m-0"><em><small>{{ data.caption }}</small></em></p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['location','data'],
  methods: {
    getImgUrl: function(name) {
      console.log('../assets/posts/' + this.location + '/images/' + name);
      return require('../assets/posts/' + this.location + '/images/' + name)
    }
  },
}
</script>

<style lang="scss">
@import "../variables";

.post-image {
  max-width: 100%;
}

</style>
