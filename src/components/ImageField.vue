<template>
  <div class="image-field">
    <ImageDiv v-if="id"
      v-bind:objectID="id"></ImageDiv>
  </div>
</template>

<script>
import ImageDiv from './ImageDiv.vue'

export default {
  name: 'ImageField',
  props: {
    options: String, //todo: api options as proper props
    amount: {
      type: Number,
      default: 5
    }
  },
  components: {
    ImageDiv
  },
  data: function(){
    return {
      id: null
    }
  },
  methods: {
    randomIndex(indexArray){
      const randomIndex = Math.floor(Math.random() * indexArray.length);  
      return indexArray.filter((el,index) => {
        return  index === randomIndex;
      })[0]
    }
  },
  created: function () {
    fetch('https://collectionapi.metmuseum.org/public/collection/v1/search' + (this.options ? '?'+this.options : ''))
      .then((res) => {
        return res.json();
      })
      .then((object) => {
        const newId = this.randomIndex(object.objectIDs);
        this.id = newId;
        console.log(this.id);
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
