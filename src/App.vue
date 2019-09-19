git <template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <ImageField options="isHighlight=true&q=yellow"/>
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
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
