<template>
  <div class="text-center text-dark pt-4 p-3">
    <post-details
      :data="data.details">
    </post-details>
    <component
      v-for="item in data.body"
      :is="item.type"
      :location='location'
      :data="item">
    </component>
  </div>
</template>

<script>
import PostDetails from './PostDetails.vue'
import PostText from './PostText.vue'
import PostHeroText from './PostHeroText.vue'
import PostImage from './PostImage.vue'
import PostButton from './PostButton.vue'

export default {
  props: ['location','data'],
  components: {
    PostDetails,
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
