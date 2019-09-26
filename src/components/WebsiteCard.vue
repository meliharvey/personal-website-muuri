<template>
  <div :id="data.postLocation" @click="recordDragEnd" :class="{ expanded: postVisible }" class="item website-item">
    <div class="item-content drop-shadow website website-card bg-white">

      <div class="website-card-header">
        <h4>{{ data.title }}</h4>
        <div class="image-container" :style="{ 'background-image': 'url(' + getImgUrl() + ')'}">
        </div>
      </div>

      <post
        v-if="postVisible"
        :location="data.postLocation"
        :data="currentPost"
        >
      </post>
    </div>
  </div>
</template>

<script>
import Post from '../components/Post.vue'

export default {
  name: "website",
  components: {
    Post
  },
  props: ['data'],
  data () {
    return {
      postVisible: false,
      currentPost: null,
      dragStart: null,
    }
  },
  methods: {
    recordDragStart: function(e) {
      this.dragStart = e;
    },
    recordDragEnd: function(e) {
      console.log('mouseup', e);
      console.log(e.clientX, this.dragStart.clientX)
      if (e.clientX != this.dragStart.clientX || e.clientY != this.dragStart.clientY ) {
        console.log('dragged');
      } else {
        this.togglePost(this.refreshGrid);
      }
    },
    getImgUrl: function() {
      return require('../assets/posts/' + this.data.postLocation + '/images/' + this.data.image)
    },
    togglePost: function(callback) {
      this.postVisible = !this.postVisible;
      this.currentPost = require('../assets/posts/' + this.data.postLocation + '/post.json')
      callback();
    },
    refreshGrid: function() {

      var self = this;
      setTimeout( function(){
        self.$parent.grid.refreshItems().layout();
      }, 1000 );

    }
  },
  mounted: function() {
    window.addEventListener('mousedown', this.recordDragStart)
  }
}
</script>

<style lang="scss">
@import "../variables";

.website-item {
  min-width: 400px;
  width: 0%;
  max-width: 960px;
  min-height: 300px;
  margin: 15px;
  box-sizing: border-box;
  transition: width .8s ease, height .8s ease;
}
.expanded {
  width: 95% !important;
}
.website-card {
  width: 100%;
  padding: 15px;
  //z-index: 1000;
}
.website-card-header {
  height: 300px;
  overflow: hidden;
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
