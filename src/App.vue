<template>
  <div id="app">
    <div class="container-fluid">
      <div class="row justify-content-end">

        <!-- Menu -->
        <div id="menuMobile" class="col-xl-2 col-lg-3 col-md-4 col-12 d-md-none bg-white fixed-top p-0">
          <div class="row m-0">

            <!-- Menu when screen size sm or xs-->
            <b-collapse id="collapse-1" class="col-12 d-md-none p-0">
              <div class="row m-0 p-3">
                <div class="col-12 text-right p-0">
                    <b-button
                      v-for="(card, key) in cards"
                      class="btn-category btn-drop-shadow m-1"
                      :class="[ card.visible ? '' : 'inactive', key]"
                      @click="filter(key)">
                      {{ card.name }}
                    </b-button>
                </div>
                <div class="col-12 text-right p-0">
                    <b-button variant="outline-dark" href="https://www.github.com/meliharvey" target='_blank' class="my-1">Github</b-button>
                    <b-button variant="outline-dark" href="https://www.instagram.com/meltyopolis/" target='_blank' class="my-1">Instagram</b-button>
                    <b-button variant="outline-dark" :href="download('MeliHarvey_CV_2023.pdf')" target='_blank' class="my-1">Resume</b-button>
                </div>
              </div>
            </b-collapse>

            <div class="col-12 d-none d-md-block p-4"></div>

            <!-- Menu-->
            <div id="brand" class="col-12 d-flex justify-content-between h-100 pt-3 px-3 m-0">
              <h2 class="flex-grow-1 text-dark m-0">Meli Harvey</h2>
              <b-button v-b-toggle.collapse-1 variant="outline-dark">Menu</b-button>
            </div>
          </div>
        </div>

        <div id="menuComputer" class="col-xl-2 col-lg-3 col-md-4 col-12 d-none d-md-block bg-white fixed-top h-100 p-0">
          <div class="row m-0">

            <div class="col-12 p-4"></div>

            <!-- Menu-->
            <div id="brand" class="col-12 d-flex justify-content-between pt-3 px-3 m-0">
              <h2 class="flex-grow-1 text-dark m-0">Meli Harvey</h2>
            </div>

            <!-- Menu when screen size is md, lg, or xl -->
            <div class="col-12 h p-3">
              <div v-for="(card, key) in cards" class="text-left">
                <b-button
                  class="btn-category btn-drop-shadow my-1"
                  :class="[ card.visible ? '' : 'inactive', key]"
                  @click="filter(key)">
                  {{ card.name }}
                </b-button>
              </div>

              <div class="p-3"></div>

              <div>
                <p class="m-0">Links</p>

                <b-button variant="outline-dark" href="https://www.github.com/meliharvey" target='_blank' class="my-1">Github</b-button>
                <br>
                <b-button variant="outline-dark" href="https://www.instagram.com/meltyopolis/" target='_blank' class="my-1">Instagram</b-button>
                <br>
                <b-button variant="outline-dark" :href="download('MeliHarvey_CV_2023.pdf')" target='_blank' class="my-1">Resume</b-button>
                
              </div>

              <!-- <div>
                <p class="m-0">Sort by:</p>
                <b-dropdown id="dropdown-1" variant="outline-dark" text="featured" class="my-1">
                  <b-dropdown-item>date</b-dropdown-item>
                  <b-dropdown-item>type</b-dropdown-item>
                  <b-dropdown-item>randomize</b-dropdown-item>
                </b-dropdown>
              </div>

              <div>
                <b-button variant="outline-dark" class="my-1">Toggle Color</b-button>
              </div> -->

            </div>

          </div>
        </div>

        <div class="col-12 d-none d-md-block p-4"></div>
        <div class="col-12 d-md-none p-2"></div>

        <!-- Project Grid -->
        <div id="main" class="col-xl-10 col-lg-9 col-md-8 col-12">
          <div class="grid m-0">
            <component
              v-for="item in data"
              :is="item.type + '-card'"
              :data="item">
            </component>
          </div>
          <div class="p-4 mt-5 text-center text-muted">
            <p><small>© 2022 Amelia Harvey | fork this website <a href="https://github.com/meliharvey/personal-website-muuri">here</a></small></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import AboutCard from './components/AboutCard.vue'
import WritingCard from './components/WritingCard.vue'
import DesignCard from './components/DesignCard.vue'
import TechCard from './components/TechCard.vue'
import data from './data/data.js'
import Muuri from 'muuri'
import animate from 'web-animations-js'

export default {
  name: 'app',
  components: {
    AboutCard,
    DesignCard,
    TechCard,
    WritingCard
  },
  data () {
    return {
      isMobile: false,
      data: data,
      grid: null,
      cards: {
        about: {
          name: 'About',
          visible: true,
          selected: false
        },
        tech: {
          name: 'Tech',
          visible: true,
          selected: false
        },
        design: {
          name: 'Design',
          visible: true,
          selected: false
        },
        writing: {
          name: 'Writing',
          visible: true,
          selected: false
        }
      },
      postVisible: false,
      currentPost: null
    }
  },
  computed: {
    order: function() {
      var projectOrder = []
      for (var i=0; i<this.data.length; i++) {
        projectOrder.push(i)
      }
      return projectOrder
    }
  },
  methods: {
    checkMobile: function() {
      var width = window.innerWidth;

      if (width <= 768) {
        this.isMobile = true;
      } else {
        this.isMobile = false;
      }
      console.log(this.isMobile)
    },
    download: function(file) {
      return require('./assets/' + file)
    },
    filter: function(type) {
      console.log(type)

      var cardTypes = [];
      var selected = this.cards[type].selected;
      var visible = this.cards[type].visible;

      if (visible == true && selected == true) {
        for (var card in this.cards) {
          if (card == type) {
            this.cards[card].visible = true;
            this.cards[card].selected = false;
            var cardType = card + '-item'
            cardTypes.push(cardType);
          } else {
            this.cards[card].visible = true;
            this.cards[card].selected = false;
            var cardType = card + '-item'
            cardTypes.push(cardType);
          }
        }
      } else if (visible == true && selected == false) {

        for (var card in this.cards) {
          if (card == type) {
            this.cards[card].visible = true;
            this.cards[card].selected = true;
            var cardType = card + '-item'
            cardTypes.push(cardType);
          } else {
            this.cards[card].visible = false;
            this.cards[card].selected = false;
          }
        }
      } else {

        for (var card in this.cards) {
          if (card == type) {
            this.cards[card].visible = true;
            this.cards[card].selected = true;
            var cardType = card + '-item'
            cardTypes.push(cardType);
          } else {
            this.cards[card].visible = false;
            this.cards[card].selected = false;
          }
        }
      }



      console.log(cardTypes);

      this.grid.filter(function (item) {
        var returnItem = false;

        cardTypes.forEach(function(value){
          if (item.getElement().classList.contains(value)) {
            returnItem = true;
          }
        })

        return returnItem;
      });

      this.grid.refreshItems().layout()
    }
  },
  created: function() {
    window.addEventListener('resize', this.checkMobile)
    this.checkMobile();
  },
  mounted: function() {

    var self = this;

    var grid = new Muuri('.grid', {
      // if a mobile device disable grid
      dragEnabled: !this.isMobile,
      dragSortHeuristics: {
        sortInterval: 0,
      },
      layout: {
        fillGaps: false,
        alignLeft: true,
        alignRight: false
      },
      layoutOnResize: 0,
      layoutDuration: 500,
    });

    // When all items have loaded refresh their
    // dimensions and layout the grid.
    window.addEventListener('load', function () {
      grid.refreshItems().layout();
      // For a little finishing touch, let's fade in
      // the images after all them have loaded and
      // they are corrertly positioned.
      document.body.classList.add('images-loaded');
    });

    this.grid = grid;

    for (var i=0; i<this.data.length; i++) {
      Vue.set(this.data[i], 'visible', true)
      Vue.set(this.data[i], 'favorited', false)
    }
  }
}
</script>

<style lang="scss">
@import "variables";

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
#menuMobile {
  min-width: 250px;
  -webkit-box-shadow: 0px 20px 20px 0px white !important;
  -moz-box-shadow: 0px 20px 20px 0px white !important;
  box-shadow: 0px 20px 20px 0px white !important;
  z-index: 2000;
}
#menuComputer {
  min-width: 250px;
  -webkit-box-shadow: 0px 20px 20px 0px white !important;
  -moz-box-shadow: 0px 20px 20px 0px white !important;
  box-shadow: 0px 20px 20px 0px white !important;
}
#brand {
  height: 60px;
  font-weight: 900px !important;
}
#main {
  margin-top: 60px;
  z-index: 1090;
}

::-webkit-scrollbar {
    display: none;
}
h4 {
  font-weight: bold;
}
b-button {
  z-index: 1000 !important;
}
.card-image {
  -webkit-filter: saturate(0%);
  filter: saturate(0%);
}

.grid {
  position: relative;
  opacity: 0;
  transition: opacity 0.8s linear 1s;
}
.images-loaded .grid {
  opacity: 1;
}
.item {
  position: absolute;
  margin: 5px;
  z-index: 1;
}
.item.muuri-item-hidden {
  z-index: 0;
}
.item.muuri-item-releasing {
  z-index: 2;
}
.item.muuri-item-dragging {
  z-index: 3;
}
.item-content {
  position: relative;
  cursor: pointer;
}
.item-content > img {
  display: block;
  //border-radius: 6px;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  text-transform: uppercase;
  transition: opacity 0.6s linear 0.4s;
}
.images-loaded .loading {
  opacity: 0;
}

.btn-category {
  background-color: white !important;
  border: solid 1px white !important;
}
.card {
  border: none;
  transition: box-shadow .15s ease-in-out;
}
.about {
  color: $color2 !important;
}
.design {
  color: $color1 !important;
}
.tech {
  color: $color3 !important;
}
.writing {
  color: $color4 !important;
}

.drop-shadow-menu {
  border: 1px solid #ffffff !important;
  -webkit-box-shadow: 0px 0px 1px 0px !important;
  -moz-box-shadow: 0px 0px 1px 0px !important;
  box-shadow: 0px 0px 1px 0px !important;
}

.drop-shadow {
  position: relative;
}

.drop-shadow:before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: -1000 !important;
  -webkit-box-shadow: 6px 6px 40px 0px !important;
  -moz-box-shadow: 6px 6px 40px 0px !important;
  box-shadow: 6px 6px 40px 0px !important;
  transition: box-shadow .15s ease-in-out;
}
.drop-shadow.about:before {
  -webkit-box-shadow: 6px 6px 40px 0px $color2 !important;
  -moz-box-shadow: 6px 6px 40px 0px $color2 !important;
  box-shadow: 6px 6px 40px 0px $color2 !important;
}
.drop-shadow.design:before {
  -webkit-box-shadow: 6px 6px 40px 0px $color1 !important;
  -moz-box-shadow: 6px 6px 40px 0px $color1 !important;
  box-shadow: 6px 6px 40px 0px $color1 !important;
}
.drop-shadow.tech:before {
  -webkit-box-shadow: 6px 6px 40px 0px $color3 !important;
  -moz-box-shadow: 6px 6px 40px 0px $color3 !important;
  box-shadow: 6px 6px 40px 0px $color3 !important;
}
.drop-shadow.writing:before {
  -webkit-box-shadow: 6px 6px 40px 0px $color4 !important;
  -moz-box-shadow: 6px 6px 40px 0px $color4 !important;
  box-shadow: 6px 6px 40px 0px $color4 !important;
}

.drop-shadow:hover:before {
  -webkit-box-shadow: 5px 5px 70px 0px !important;
  -moz-box-shadow: 5px 5px 70px 0px !important;
  box-shadow: 5px 5px 70px 0px !important;
}

.btn:focus {
  box-shadow: none !important;
}
.btn:active {
  box-shadow: none !important;
}
.btn-drop-shadow {
  position: relative;
}
.btn-drop-shadow:before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: -1000 !important;
  -webkit-box-shadow: 6px 6px 50px 0px !important;
  -moz-box-shadow: 6px 6px 50px 0px !important;
  box-shadow: 6px 6px 50px 0px !important;
  transition: box-shadow .15s ease-in-out;
}
//box-shadow color doesn't default to color in safari
.btn-drop-shadow.about:before {
  -webkit-box-shadow: 6px 6px 50px 0px $color2 !important;
  -moz-box-shadow: 6px 6px 50px 0px $color2 !important;
  box-shadow: 6px 6px 50px 0px $color2 !important;
}
.btn-drop-shadow.design:before {
  -webkit-box-shadow: 6px 6px 50px 0px $color1 !important;
  -moz-box-shadow: 6px 6px 50px 0px $color1 !important;
  box-shadow: 6px 6px 50px 0px $color1 !important;
}
.btn-drop-shadow.tech:before {
  -webkit-box-shadow: 6px 6px 50px 0px $color3 !important;
  -moz-box-shadow: 6px 6px 50px 0px $color3 !important;
  box-shadow: 6px 6px 50px 0px $color3 !important;
}
.btn-drop-shadow.writing:before {
  -webkit-box-shadow: 6px 6px 50px 0px $color4 !important;
  -moz-box-shadow: 6px 6px 50px 0px $color4 !important;
  box-shadow: 6px 6px 50px 0px $color4 !important;
}
.btn-drop-shadow:hover:before {
  -webkit-box-shadow: 5px 5px 15px 0px !important;
  -moz-box-shadow: 5px 5px 15px 0px !important;
  box-shadow: 5px 5px 15px 0px !important;
}
.inactive {
  color: black !important;
  border: solid 1px #343a40 !important;
}
.btn-drop-shadow.inactive:before {
  -webkit-box-shadow: 0px 0px 0px 0px !important;
  -moz-box-shadow: 0px 0px 0px 0px !important;
  box-shadow: 0px 0px 0px 0px !important;
}
.btn-drop-shadow.inactive:hover:before {
  -webkit-box-shadow: 1px 1px 4px !important;
  -moz-box-shadow: 1px 1px 4px !important;
  box-shadow: 1px 1px 4px !important;
}


</style>
