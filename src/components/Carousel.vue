<template>
  <div class="root">
    <div class="scene" :style="{perspective: config.perspective+'px' }">
      <div class="carousel" v-if="posts && posts.length" :style="carouselStyle">
         <Cell :post="post" :index="index" v-for="(post, index) of postLimit" :style="cellStyle(index)" />
      </div>      
    </div>


    <div class="controls">
      <label for="rotateY">rotateY</label>
      <input type="range" max="360" name="rotateY" v-model.number="config.rotateY">
      
      <label for="rotateX">rotateX</label>
      <input type="range" max="360" name="rotateX" v-model.number="config.rotateX">
      
      <label for="perspective">perspective</label>
      <input type="range" max="5000" name="perspective" v-model.number="config.perspective">
      
      <label for="itemCount">itemCount</label>
      <input type="range" max="30" name="itemCount" v-model.number="config.itemCount">

      <ul v-if="errors && errors.length">
        <li v-for="error of errors">
          {{error.message}}
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
import axios from "axios";
import Cell from "./Cell.vue";

export default {
  name: "Carousel",
  props: {
    msg: String
  },
  components: { Cell },
  data() {
    return {
      baseUrl: process.env.BASE_URL,
      posts: [],
      errors: [],
      config: {
        itemCount: 16,
        rotateX: 8,
        rotateY: 0,
        perspective: 1000,
        cellSize: 210 + 20
      }
    };
  },
  computed: {
    postLimit: function() {
      return this.posts.slice(1, this.config.itemCount + 1);
    },
    translateZ: function() {
      return Math.round(
        this.config.cellSize / 2 / Math.tan(Math.PI / this.config.itemCount)
      );
      // return Math.round(
      //   this.config.cellSize /
      //     2 /
      //     Math.tan(this.radians(360 / this.config.itemCount / 2))
      // );
    },
    carouselStyle: function() {
      return {
        transform: `translateZ(-${this.translateZ}px) rotateX(-${
          this.config.rotateX
        }deg) rotateY(${this.config.rotateY}deg)`
      };
    }
  },
  methods: {
    cellStyle: function(index) {
      let rotateY = index * (360 / this.config.itemCount);
      return {
        transform: `rotateY(${rotateY}deg) translateZ(${this.translateZ}px)`,
        opacity: 0.25 + this.cellOpacity(index) / (4 / 3)
      };
    },
    getTanFromDegrees(degrees) {
      return Math.tan((degrees * Math.PI) / 180);
    },
    radians(degrees) {
      return (degrees * Math.PI) / 180;
    },
    cellOpacity(index) {
      // y = amplitude * cos(x) + shiftY
      return (
        0.5 *
          Math.cos(
            this.radians((360 * index) / this.config.itemCount) +
              this.radians(this.config.rotateY)
          ) +
        0.5
      );
    }
  },
  created() {
    // axios
    // .get(`http://learningresources.localhost/resources-json/`) // nope no CORS
    // .then(response => {
    //   // JSON responses are automatically parsed.
    //   this.posts = response.data;
    // })
    // .catch(e => {
    //   this.errors.push(e);
    // });
    this.posts = require(`../assets/resources.json`);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.root {
  display: grid;
  grid-template-columns: 4fr 1fr;
}
.scene {
  // border: 1px solid #ccc;
  // margin: 40px 0;
  position: relative;
  width: 210px;
  height: 100%;
  margin: 80px auto;
  perspective: 1000px;
}

.carousel {
  width: 100%;
  height: 100%;
  position: absolute;
  // transform: translateZ(-288px);
  transform-style: preserve-3d;
  transition: transform 1s;
}

.controls {
  display: flex;
  flex-direction: column;
  padding: 1em;
}
</style>
