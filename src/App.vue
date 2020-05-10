<template>
  <div id="app">
  <div class="row">
      <div class="col-6">
        <h1>
        con embeded
        </h1>
          <pdf></pdf>
      </div>

      <div class="col-6">
      <h1>
      con cancas
      </h1>
        <canbas></canbas>
      </div>
  </div>
  <div>

  </div>


  </div>
</template>

<script>
import pdf from '@/components/WebViewer.vue'
import canbas from '@/components/Canvasviewer.vue'
 
export default {
  name: 'App',
  components: {
    pdf,
    canbas
  },

  mounted() {

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
