<template>
  <div id="app">
    <div id="main"></div>
  </div>
</template>

<script>
import pdfjsLib from 'pdfjs-dist'
export default {
  name: 'app',
  data () {
    return {
      pageNum: null,
      url: 'http://www.axmag.com/download/pdfurl-guide.pdf',
      pdfData: null,
      narrow: false
    }
  },
  methods: {
    createPdfContainer (id) {
      const pdfContainer = document.getElementById('main')
      const newCanvas = document.createElement('canvas')
      newCanvas.id = id
      pdfContainer.appendChild(newCanvas)
    },
    createSeriesCanvas (num, template) {
      let id
      for (let i = 1; i < num + 1; i++) {
        id = template + i
        this.createPdfContainer(id)
      }
    },
    async loadPDF () {
      const pdf = await pdfjsLib.getDocument(this.url)
      this.pdfData = pdf
      let id = null
      const idTemplate = 'pdf-'
      this.pageNum = pdf.numPages
      this.createSeriesCanvas(this.pageNum, idTemplate)
      for (let i = 1; i <= this.pageNum; i++) {
        id = idTemplate + i
        this.renderPDF(pdf, i, id)
      }
    },
    async renderPDF (pdf, i, id, scale = 2) {
      const page = await pdf.getPage(i)
      const viewport = page.getViewport(scale)
      const canvas = document.getElementById(id)
      const context = canvas.getContext('2d')
      canvas.height = viewport.height
      canvas.width = viewport.width
      canvas.addEventListener('click', this.zoom)
      const renderContext = {
        canvasContext: context,
        viewport: viewport
      }
      page.render(renderContext)
    },
    zoom (e) {
      const canvas = e.target
      if (!this.narrow) {
        canvas.style.transformOrigin = '0 0'
        canvas.style.transform = 'scale(1.5)'
        this.narrow = true
      } else {
        canvas.style.transform = 'scale(1)'
        this.narrow = false
      }
    }
  },
  mounted () {
    this.loadPDF()
  }
}
</script>

<style>
#main {
  width: 100%;
  overflow: scroll
}
</style>
