<template>
  <div class="image-field">
    <ImageDiv
      v-for="objectID in ids"
      v-bind:objectID="objectID"
      v-bind:key="objectID.id"
    ></ImageDiv>
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
      ids: []
    }
  },
  created: function () {
    fetch('https://collectionapi.metmuseum.org/public/collection/v1/search' + (this.options ? '?'+this.options : ''))
      .then((res) => {
        return res.json();
      })
      .then((object) => {
        const newIds = object.objectIDs.filter((el,index) => {
          return index < this.amount;
        });
        this.ids = newIds;
        console.log(newIds);
      })
      .catch((error)=> {
        console.error(error);
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
