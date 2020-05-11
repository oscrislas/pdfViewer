<template>
  <div> 
    <button @click="mescla">merge</button>
<object data="http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf" type="application/pdf">
  <embed src="https://docs.google.com/viewer?url=your_url_to_pdf&embedded=true" type="application/pdf" />
  
</object>

  </div>
</template>

<script>
import  PDFMerger from 'pdf-merger-js'
var merger = new PDFMerger()
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
  methods:{
async mescla() {
  merger.add('http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf');  //merge all pages. parameter is the path to file and filename.
  merger.add('https://cdn.rawgit.com/mozilla/pdf.js/c6e8ca86/test/pdfs/calrgb.pdf', [2]); // merge only page 2
 
  await merger.save('merged.pdf'); //save under given name
}
  }



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