<template>
  <div ref="wrapper" class="art-wrapper">
    <div class="art-description vertical-center">
      <div class="art-plaque">
        <h3 v-if="artist" class="art-text-artist">{{artist}}<span v-if="artistBio"> ({{artistBio}})</span></h3>
        <h3 class="art-text-title">{{title}}<span>, {{date}}</span></h3>
        <p v-if="medium" class="art-text-medium">{{medium}}</p>
        <p>{{department}}</p>
        <button @click="remove">Remove artwork</button>
      </div>
    </div>
    <div class="image-container">
      <div ref="imagewrapper" class="image-wrapper vertical-center">
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
      artist: null,
      artistBio: null,
      medium: null,
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
        imagewrapper.clientWidth-160,
        imagewrapper.clientHeight-160);
      
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
      
      TweenLite.to(wrapper, 1, {xPercent:-200, onComplete:() => {
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
        this.artist = object.artistDisplayName;
        this.medium = object.medium;
        this.artistBio = object.artistDisplayBio;
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
  min-height: 800px;
}
.art-wrapper{
  display:flex;
  transform: translateX(100%);
  padding: 0 100px;
}
.art-description{
  flex: 1;
}
.art-plaque{
  width: 400px;
  background-color: white;
  box-shadow: 0px 5px 20px 1px rgba(0,0,0,0.05);
  text-align: left;
  padding: 20px;
  padding-bottom: 100px;
  font-size: 10px;
}
.art-plaque h3{
  margin-top: 0px;
  margin-bottom: 5px;
  font-size: 16px;
}
.art-text-title{
  font-style: italic;
}
h3 span{
  font-weight: normal;
}
.art-text-medium{
  font-size: 12px;
  margin: 20px 0;
}
.image-wrapper img{
  border: 80px solid;
  border-image: url("../assets/frame_asset.png") 80 repeat;
  box-shadow: 0px 5px 20px 1px rgba(0,0,0,0.2);
}
.vertical-center{
  display: flex;
  flex-flow: column;
  justify-content: center;
}
.fixed{
  position: fixed;
}
</style>
