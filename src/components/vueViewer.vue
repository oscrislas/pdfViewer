<template>
  <div>
    <pdf :src="src" style="display: inline-block; width: 100%" :page="i" v-for=" i of numPages" :key="i"  ></pdf>  
  </div>
</template>

<script>
import pdf from 'vue-pdf'

export default {
  name: 'viwerPDF',
  props:['address'],
  components: {      
        pdf
    },
    data(){
      return{
        src: null,
        numPages: undefined,
      }
    },
    mounted() {
      this.src = pdf.createLoadingTask(this.address)
      
      this.src.then(pdf => {
        this.numPages = pdf.numPages;
      })
 
	}
}
</script>

<style scoped>

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
