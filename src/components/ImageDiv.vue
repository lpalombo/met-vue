<template>
  <div class="image-square">
    <div v-if="loading">
      <p>Loading...</p>
    </div>
    <div  v-show="!loading" >
      <img ref="metImage" :src="imageURL" @load="loaded">
    </div>
   
  </div>
</template>

<script>
import { TimelineLite, Back } from 'gsap'

export default {
  name: 'ImageDiv',
  props: {
    objectID: Number,
  },
  data: function() {
    return {
      imageURL: null,
      loading: true
    }
  },
  methods: {
    loaded() {
      this.loading = false;
      const { metImage } = this.$refs;
      const timeline = new TimelineLite()

      timeline.to(metImage, 1, {
        rotation: 90,
        ease: Back.easeInOut, // Specify an ease
      })
    }
  },
  mounted: function () {
    fetch('https://collectionapi.metmuseum.org/public/collection/v1/objects/' + this.objectID)
      .then((res) => {
        return res.json();
      })
      .then((object) => {
        console.log(object); //don't render element until image finishes loading
        this.imageURL = object.primaryImage;
      })
      .catch((error)=> {
        console.error(error);
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.image-square{
  width: 100px;
  height: 100px;
  position: relative;
}
.image-square img{
  height: 100px;
}
</style>
