<template>
  <div ref="wrapper" class="art-wrapper">
    <div class="art-description">
      <h3>{{title}}</h3>
      <p>{{department}}</p>
      <p>{{date}}</p>
      <button @click="remove">Remove artwork</button>
    </div>
    <div class="image-container">
      <div ref="imagewrapper" class="image-wrapper">
        <div v-if="loading">
          <p>Loading...</p>
        </div>
        <img v-show="!loading" ref="metImage" :src="imageURL" @load="loaded">
      </div>
    </div>
  </div>
</template>

<script>
import { TimelineLite, Back, TweenLite } from 'gsap'

export default {
  name: 'ImageDiv',
  props: {
    objectID: Number,
  },
  data: function() {
    return {
      imageURL: null,
      loading: true,
      title: null,
      department: null,
      date: null,
      imageDimensions: {
        w: null,
        h: null,
        ratio: null
      }
    }
  },
  methods: {
    loaded() {
      this.loading = false;
      const { metImage, imagewrapper } = this.$refs;

      //populate image dimensions
      this.imageDimensions.w = metImage.width;
      this.imageDimensions.h = metImage.height;
      this.imageDimensions.ratio = metImage.width/metImage.height;

      const newDimensions = this.calculateAspectRatioFit(
        metImage.width,
        metImage.height,
        imagewrapper.clientWidth-100,
        imagewrapper.clientHeight-100);
      
      console.log(newDimensions);
      metImage.width = newDimensions.width;
      metImage.height = newDimensions.height;

      this.$emit('loaded');
      this.animateIn();
    },
    animateIn() {
      const { wrapper } = this.$refs
      TweenLite.to(wrapper, 1, {xPercent:-100}); 
    },
    calculateAspectRatioFit(srcWidth, srcHeight, maxWidth, maxHeight) {
      var ratio = Math.min(maxWidth / srcWidth, maxHeight / srcHeight);
      return { width: srcWidth*ratio, height: srcHeight*ratio }; 
    },
    remove(){
      const { wrapper } = this.$refs
      
      TweenLite.to(wrapper, 1, {xPercent:-200, onComplete:()=> {
        this.$emit('remove');
      }}); 
    }
  },
  computed: {
    newImageDimensions: function (){
      return null //change
    }
  },
  mounted: function () {
    fetch('https://collectionapi.metmuseum.org/public/collection/v1/objects/' + this.objectID)
      .then((res) => {
        return res.json();
      })
      .then((object) => {
        console.log(object);
        this.imageURL = object.primaryImageSmall;
        this.department = object.department;
        this.title = object.title;
        this.date = object.objectDate;
      })
      .catch((error)=> {
        console.error(error);
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.image-container{
  flex: 1;
}
.image-wrapper{

  box-sizing: border-box;
  height: 800px;
  background-color: red;
}
.art-wrapper{
  display:flex;
  transform: translateX(100%);
}
.art-description{
  flex: 1;
}
.image-wrapper img{

}
.fixed{
  position: fixed;
}
</style>
