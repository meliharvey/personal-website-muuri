<template>
  <div @click="recordDragEnd" class="item about-item">
    <div class="item-content drop-shadow about card about-card active p-5">

      <div class="about-card-header">
        <div v-if="this.data.image" class="about-image pr-3">
          <div class="image-container" :style="{ 'background-image': 'url(' + image + ')'}">
          </div>
        </div>
        <div class="">
          <h4>{{ data.title }}</h4>
          <p class="mb-0">{{ data.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "about",
  props: ['data'],
  data () {
    return {
      dragStart: null,
      image: '',
    }
  },
  methods: {
    getImgUrl: function() {
      if (this.data.image) {
        this.image = require('../assets/' + this.data.image)
      }
    },
    recordDragStart: function(e) {
      this.dragStart = e;
    },
    recordDragEnd: function(e) {
      console.log('mouseup', e);
      console.log(e.clientX, this.dragStart.clientX)
      if (e.clientX != this.dragStart.clientX || e.clientY != this.dragStart.clientY ) {
        console.log('dragged');
      } else {
        this.$parent.filter(this.$options.name);
      }
    },
  },
  mounted: function() {
    this.getImgUrl()
    window.addEventListener('mousedown', this.recordDragStart)
  }
}
</script>

<style lang="scss">
@import "../variables";

// .about-card {
//   max-width: 700px;
// }

.about-item {
  min-width: 300px;
  width: 95%;
  max-width: 800px;
  min-height: 200px;
  margin: 15px;
  box-sizing: border-box;
  transition: width .8s ease, height .8s ease;
}
.expanded {
  width: 95% !important;
}
.about-card {
  width: 100%;
}
// .about-card-header {
//   height: 180px;
//   overflow: auto;
// }
.about-image {
  float: left;
  width: 200px;
  height: 180px;
}
.image-container {
  width: 100%;
  height: 100%;
  background-position: center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

</style>
