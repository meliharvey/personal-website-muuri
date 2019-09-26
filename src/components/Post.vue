<template>
  <div class="text-center text-dark pt-5 p-3">
    <component
      v-for="item in data.body"
      :is="item.type"
      :location='location'
      :data="item">
    </component>
  </div>
</template>

<script>
import PostText from './PostText.vue'
import PostHeroText from './PostHeroText.vue'
import PostImage from './PostImage.vue'
import PostButton from './PostButton.vue'

export default {
  props: ['location','data'],
  components: {
    PostText,
    PostHeroText,
    PostImage,
    PostButton
  }
}
</script>

<style lang="scss">
@import "../variables";

</style>
