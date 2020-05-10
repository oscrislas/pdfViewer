<template>
  
  <div>
    <h1>
      hijo 
    </h1>
    <pdf src="/static/1.pdf" style="display: inline-block; width: 100%" :page="1"></pdf>
  </div>

</template>

<script>

import pdf from 'vue-pdf'
export default {
  name: 'viwerPDF',
  components: {
        pdf
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
