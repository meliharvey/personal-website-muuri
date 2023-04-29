<template>
  <div :id="data.postLocation" @click="recordDragEnd" :class="{ 'expanded': postVisible }" class="item design-item">
    <div class="item-content drop-shadow design design-card bg-white">

      <div :class="{ 'design-card-header-expanded': postVisible }" class="design-card-header">
        <h4>{{ data.title, data.visible }}</h4>
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
  name: "design",
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
    getImgUrl: function(file) {
      this.image = require('../assets/posts/' + this.data.postLocation + '/images/' + this.data.image)
    },
    togglePost: function(callback) {
      if (this.postVisible) {
        this.scrollToTop(1000);
      }
      this.postVisible = !this.postVisible;
      this.currentPost = require('../assets/posts/' + this.data.postLocation + '/post.json')
      callback();
    },
    refreshGrid: function() {

      var self = this;
      setTimeout( function(){
        self.$parent.grid.refreshItems().layout();
      }, 1000 );

    },
    scrollToTop: function(scrollDuration) {
    const   scrollHeight = window.scrollY,
            scrollStep = Math.PI / ( scrollDuration / 15 ),
            cosParameter = scrollHeight / 2;
    var     scrollCount = 0,
            scrollMargin,
            scrollInterval = setInterval( function() {
                if ( window.scrollY != 0 ) {
                    scrollCount = scrollCount + 1;
                    scrollMargin = cosParameter - cosParameter * Math.cos( scrollCount * scrollStep );
                    window.scrollTo( 0, ( scrollHeight - scrollMargin ) );
                }
                else clearInterval(scrollInterval);
            }, 15 );
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

img {
  mix-blend-mode: multiply;
}

.design-item {
  min-width: 300px;
  width: 95%;
  max-width: 300px;
  min-height: 300px;
  margin: 15px;
  box-sizing: border-box;
  transition: max-width .6s ease, height .6s ease;
}
.expanded {
  max-width: 900px;
}
.design-card {
  width: 100%;
  padding: 15px;
  //z-index: 1000;
}
.design-card-header {
  height: 300px;
  overflow: hidden;
  transition: width .5s ease, height .5s ease;
}
.design-card-header-expanded {
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
