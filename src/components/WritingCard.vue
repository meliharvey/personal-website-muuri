<template>
  <div @click="recordDragEnd" class="item writing-item">
    <div class="item-content drop-shadow writing writing-card bg-white p-4">
      <div class="writing-card-header">
        <div class="writing-image pr-3">
          <div class="image-container" :style="{ 'background-image': 'url(' + image + ')'}">
          </div>
        </div>
        <div>
          <h4>{{ data.title }}</h4>
          <p>{{ data.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "writing",
  props: ['data'],
  data () {
    return {
      dragStart: null,
      image: '',
    }
  },
  methods: {
    getImgUrl: function() {
      this.image = require('../assets/posts/' + this.data.postLocation + '/images/' + this.data.image)
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

.writing-item {
  min-width: 300px;
  width: 95%;
  max-width: 960px;
  min-height: 200px;
  margin: 15px;
  box-sizing: border-box;
  transition: width .8s ease, height .8s ease;
}
.expanded {
  width: 95% !important;
}
.writing-card {
  width: 100%;
}
.writing-card-header {
  // height: 180px;
  overflow: auto;
}
.writing-image {
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
