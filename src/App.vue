<template>
  <div id="app">
  <div class="row">
      <div class="col-12 ">
        <h1>
        con embeded
        </h1>
          <pdf class="medida"></pdf>
      </div>

      <div class="col-12 ">
      <h1>
      con vue-pdf
      <multiple class="medida"/>
      </h1>

      </div>

      <div class="col-12 ">
      <h1>
      con canvas
      </h1>
        <canbas class="medida"></canbas>
      </div>

  </div>
  <div>

  </div>


  </div>
</template>

<script>
import pdf from '@/components/WebViewer.vue'
import canbas from   '@/components/Canvasviewer.vue'
import multiple from '@/components/multiple.vue'
 
export default {
  name: 'App',
  components: {
    pdf,
    canbas,
    multiple
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
.medida{
  margin: auto;
  width: 900px;
  height: 1200px;
}
</style>
