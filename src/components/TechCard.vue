<template>
  <div :id="data.postLocation" @click="recordDragEnd" :class="{ expanded: postVisible }" class="item tech-item">
    <div class="item-content drop-shadow tech tech-card bg-white">

      <div :class="{ 'tech-card-header-expanded': postVisible }" class="tech-card-header">
        <h4>{{ data.title }}</h4>
        <div class="image-container" :style="{ 'background-image': 'url(' + image + ')'}">
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
  name: "tech",
  components: {
    Post
  },
  props: ['data'],
  data () {
    return {
      postVisible: false,
      currentPost: null,
      dragStart: null,
      image: '',
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
      this.image = require('../assets/posts/' + this.data.postLocation + '/images/' + this.data.image)
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
    this.getImgUrl()
    window.addEventListener('mousedown', this.recordDragStart)
  }
}
</script>

<style lang="scss">
@import "../variables";

.tech-item {
  min-width: 300px;
  width: 95%;
  max-width: 400px;
  min-height: 300px;
  margin: 15px;
  box-sizing: border-box;
  transition: max-width .6s ease, height .6s ease;
}
.expanded {
  max-width: 900px;
}
.tech-card {
  width: 100%;
  padding: 15px;
  //z-index: 1000;
}
.tech-card-header {
  height: 300px;
  overflow: hidden;
  transition: width .8s ease, height .8s ease;
}
.tech-card-header-expanded {
  height: 550px !important;
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
