<template>
  <div :id="data.postLocation" @click="recordDragEnd" :class="{ expanded: postVisible }" class="item writing-item">
    <div class="item-content drop-shadow writing writing-card bg-white p-4">

      <div class="writing-card-header">
        <div class="writing-image pr-3">
          <div class="image-container" :style="{ 'background-image': 'url(' + getImgUrl() + ')'}">
          </div>
        </div>
        <div class="">
          <h4>{{ data.title }}</h4>
          <p>{{ data.body }}</p>
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
  name: "writing",
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

.writing-item {
  min-width: 400px;
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
  height: 180px;
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
