<template>
  <div ref="field" class="image-field">
    <button @click="createNewImage">Add new art piece</button>
    <ImageDiv
      v-for="(Image, index) in images"
      v-bind:objectID="Image.objectId"
      v-bind:key="Image.id"
      @remove="images.splice(index, 1)"
    >
    </ImageDiv>
  </div>
</template>

<script>
import ImageDiv from './ImageDiv.vue'

export default {
  name: 'ImageField',
  props: {
    options: String //todo: api options as proper props
  },
  components: {
    ImageDiv
  },
  data: function(){
    return {
      ids: [],
      images: [],
      nextImageId: 0
    }
  },
  methods: {
    randomIndex(indexArray){
      const randomIndex = Math.floor(Math.random() * indexArray.length);  
      return indexArray.filter((el,index) => {
        return  index === randomIndex;
      })[0]
    },
    createNewImage(){
      if(this.ids.length)
        this.images.push(
          {
            id: this.nextImageId++,
            objectId: this.randomIndex(this.ids)
          });
      else{
        console.error("ID array currently empty");
      }
    },
    removeElement: function (index) {
      this.$delete(this.images,index);
      console.log(index);
    }
  },
  created: function () {
    fetch('https://collectionapi.metmuseum.org/public/collection/v1/search' + (this.options ? '?'+this.options : ''))
      .then((res) => {
        return res.json();
      })
      .then((object) => {
        //const newId = this.randomIndex(object.objectIDs);
        this.ids = object.objectIDs;
      })
      .catch((error)=> {
        console.error(error);
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.image-field{
  margin: 0 auto;
}
</style>
