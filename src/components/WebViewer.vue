<template>
  <div> 

<object data="http://www.pkasesorias.cl/imagenes/CARS%20ESPANOL.pdf" type="application/pdf">
  <embed src="https://docs.google.com/viewer?url=your_url_to_pdf&embedded=true" type="application/pdf" />
  
</object>
    
  </div>
</template>

<script>

export default {
  name: "vuepdf",
  components: {},
  data() {
    return {
      currPage : 1,
      numPages : 0,
      thePDF : null
    }
  },
  created() {
    console.log("entro a create")

  },



}
</script>

<style scoped>
object{
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  width: 500px;
  height: 500px;
}

::-webkit-scrollbar {
    display: none;  /* Ocultar scroll */
}
</style>