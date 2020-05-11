<template>
  <div> 
    <button @click="mescla">merge</button>
    <object data="http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf" type="application/pdf">
      <embed src="https://docs.google.com/viewer?url=your_url_to_pdf&embedded=true" type="application/pdf" />
    </object>
  </div>
</template>

<script>

export default {
  name: "vuepdf",
}
</script>

<style scoped>
object{
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  width: 100%;
  height: 100%;
}

::-webkit-scrollbar {
    display: none;  /* Ocultar scroll */
}
</style>