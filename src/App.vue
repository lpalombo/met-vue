git <template>
  <div id="app">
    <ImageField options="isHighlight=true&hasImages=true&medium=Paintings&q=portrait"/>
  </div>
</template>

<script>
import ImageField from './components/ImageField.vue'

export default {
  name: 'app',
  components: {
    ImageField
  }
}
</script>

<style>
body, html {
  height: 100%;
  width: 100%;
}
body{
  background: rgb(236,236,236);
  background: linear-gradient(0deg, rgba(236,236,236,1) 0%, rgba(255,255,255,1) 100%);
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
