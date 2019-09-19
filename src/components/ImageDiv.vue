<template>
  <div class="image-square">
    <div v-if="loading">
      <p>fuck offs!</p>
    </div>
    <div v-else>
       <img v-bind:src="imageURL">
    </div>
   
  </div>
</template>

<script>
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
  mounted: function () {
    fetch('https://collectionapi.metmuseum.org/public/collection/v1/objects/' + this.objectID)
      .then((res) => {
        return res.json();
      })
      .then((object) => {
        console.log(object); //don't render element until image finishes loading
        this.imageURL = object.primaryImage;
        this.loading = false;
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
}
.image-square img{
  height: 100px;
}
</style>
