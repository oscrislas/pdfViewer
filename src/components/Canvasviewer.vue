<template>
  <div> 
    <div class="tam">
      <canvas id="the-canvas" style=" border:1px solid black;" :width="width" :height="height"></canvas>
    </div>
    <div>
      <div>
        <button v-on:click="pageNum--">Previous</button>
        <button v-on:click="pageNum++">Next</button>
        <button v-on:click="scale=scale+0.1">Zoom +</button>
        <button v-on:click="scale=scale-0.1">Zoom -</button>
        <p class="text-dark">Page: {{pageNum}} / {{totalPageCount}}</p>
      </div>
    </div>
  </div>
</template>

<script>
import PDFJS from 'pdfjs-dist';

export default {
  name: "canbas",
  components: {},
  data() {
    return {
      urls : [
        'http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf',
        'http://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf'
    ],
      pdfDocs : [],
      loadedCount : 0,
      totalPageCount : 0,
      canvas : document.getElementById('the-canvas'),
      ctx : null,
      vueCanvas: null,
      rectWidth: 0,
      pageRendering: false,
      current :{},
      scale : 1,
      pageNumPending : null,
      pageNum : 1,
      width: 100,
      height: 100
    }
  },
  mounted() {
    var canvas = document.getElementById('the-canvas')
    var ctx= canvas.getContext('2d')
    this.vueCanvas = ctx
    this.load()
  },
  watch:{
    pageNum(){
      if (this.pageNum >= this.totalPageCount && this.current.documentIndex + 1 === this.pdfDocs.length) {
         this.pageNum=this.totalPageCount
          return ;
      }
      if (this.pageNum <= 1) {
        this.pageNum=1
            return;
      }
      this.queueRenderPage(this.pageNum);
    },
    scale(){
      this.queueRenderPage(this.pageNum);
    },
  },
  methods: {
   load() {
    PDFJS.getDocument(this.urls[this.loadedCount])
    .promise.then(pdfDoc_=>{    
        this.pdfDocs.push(pdfDoc_);
        this.loadedCount++;
        if (this.loadedCount !== this.urls.length) {
            return this.load();
        } 
        this.totalPageCount = this.getTotalPageCount();
        this.renderPage(this.pageNum);
    });
   },

   getTotalPageCount() {
    var totalPageCount = 0;
    for (var docIdx = 0; docIdx < this.pdfDocs.length; docIdx++) {
        totalPageCount += this.pdfDocs[docIdx].numPages;
    }
    return totalPageCount;
   },
   
   getPageInfo (num) {
    var totalPageCount = 0;
    for (var docIdx = 0; docIdx < this.pdfDocs.length; docIdx++) {
        totalPageCount += this.pdfDocs[docIdx].numPages;
        if (num <= totalPageCount) {
            return {documentIndex: docIdx, pageNumber: num};
        }
        num -= this.pdfDocs[docIdx].numPages;
    }
    return false;
   },

   renderPage(num) {
    this.pageRendering = true;
    this.current = this.getPageInfo(num);
    // Using promise to fetch the page
    this.pdfDocs[this.current.documentIndex].getPage(this.current.pageNumber).then(page => {
      var viewport = page.getViewport({scale: this.scale});
      this.vueCanvas.width=viewport.width
      this.vueCanvas.height=viewport.height
      this.height=viewport.height
      this.width=viewport.width
      // Render PDF page into canvas context
      var renderContext = {
          canvasContext: this.vueCanvas,
          viewport: viewport
      };
      var renderTask = page.render(renderContext);
          // Wait for rendering to finish
      renderTask.promise.then(() =>{
        this.pageRendering = false;
        if (this.pageNumPending !== null) {
            // New page rendering is pending
            this.renderPage(this.pageNumPending);
            this.pageNumPending = null;
        }
      });
    });
   },

    queueRenderPage(num) {
      if (this.pageRendering) {
          this.pageNumPending = num;
      } else {
          this.renderPage(num);
      }
    }
  },
}
</script>

<style scoped>
.tam{
    display:block;
    overflow: auto;
    background: #333;
    text-align: center;
    border: solid 3px;
  }

</style>>

