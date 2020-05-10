<template>
  <div class="tam"> 
    <canvas id="the-canvas" style="border:1px solid black"></canvas>
            <div>
            <button v-on:click="addWidth">Previous</button>
            <button v-on:click="load">Next</button>
            &nbsp; &nbsp;
            <span>Page: <span id="page_num"></span> / <span id="page_count"></span></span>
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
      scale : 0.8,
      pageNumPending : null,
      pageNum : 1
    }
  },
  mounted() {
    var canvas = document.getElementById('the-canvas')
    var ctx= canvas.getContext('2d')
    this.vueCanvas = ctx


  },
  methods: {
        addWidth() {
      this.rectWidth += 10
      this.drawRect()
    },
    subWidth() {
      this.rectWidth -= 10
      this.drawRect()      
    },
      drawRect() {
      // clear canvas
      this.vueCanvas.clearRect(0, 0, 900, 900);
      
      
      // draw rect
      this.vueCanvas.beginPath();
      this.vueCanvas.rect(this.rectWidth, 0, this.rectWidth+20, 20);
      this.vueCanvas.fill()
      this.vueCanvas.stroke();    
        
    },
   load() {
    // Load PDFs one after another
    var _this=this
    PDFJS.getDocument(_this.urls[_this.loadedCount])
    .promise.then(function (pdfDoc_) {
        console.log('loaded PDF ' + _this.loadedCount);
        _this.pdfDocs.push(pdfDoc_);
        _this.loadedCount++;
        if (_this.loadedCount !== _this.urls.length) {
          console.log("entro al if")
            return _this.load();
        } 
        
        console.log('Finished loading');
        _this.totalPageCount = _this.getTotalPageCount();
        document.getElementById('page_count').textContent = _this.totalPageCount;

        // Initial/first page rendering
        _this.renderPage(_this.pageNum);
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
      var _this= this

      console.log("entro a render")

    _this.pageRendering = true;
    _this.current = this.getPageInfo(num);
    // Using promise to fetch the page
    _this.pdfDocs[_this.current.documentIndex].getPage(_this.current.pageNumber).then(function (page) {
        var viewport = page.getViewport({scale: _this.scale});
        console.log( _this.current)
        
        _this.setcanvas(viewport.height,viewport.width)
        

        // Render PDF page into canvas context
        var renderContext = {
            canvasContext: _this.vueCanvas,
            viewport: viewport
        };
        var renderTask = page.render(renderContext);

        // Wait for rendering to finish
        renderTask.promise.then(function () {
            _this.pageRendering = false;
            if (_this.pageNumPending !== null) {
                // New page rendering is pending
                this.renderPage(_this.pageNumPending);
                _this.pageNumPending = null;
            }
        });
    });

    // Update page counters
     document.getElementById('page_num').textContent = this.pageNum;
},
setcanvas(width,height){
  console.log("sent vue canvas")
  this.vueCanvas.width=width
  this.vueCanvas.height=height
}


    
  },


}
</script>

<style scoped>
.tam{
  width: 100%;
  height: 500px;
  background-color: black;
}
</style>>

