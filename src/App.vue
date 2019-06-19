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
      page: 1,
      url: 'http://www.axmag.com/download/pdfurl-guide.pdf'
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
    loadPDF () {
      pdfjsLib.getDocument(this.url).then(pdf => {
        let id
        const idTemplate = 'pdf-'
        const pageNum = pdf.numPages
        this.createSeriesCanvas(pageNum, idTemplate)
        for (let i = 1; i <= pageNum; i++) {
          id = idTemplate + i
          this.renderPDF(pdf, i, id)
        }
      })
    },
    renderPDF (pdf, i, id) {
      pdf.getPage(i).then(page => {
        const scale = 2
        const viewport = page.getViewport(scale)
        const canvas = document.getElementById(id)
        const context = canvas.getContext('2d')
        canvas.height = viewport.height
        canvas.width = viewport.width
        canvas.style.width = '100%'

        const renderContext = {
          canvasContext: context,
          viewport: viewport
        }
        page.render(renderContext)
      })
    }
  },
  mounted () {
    this.loadPDF()
  }
}
</script>

<style>
#main {
  width: 100%
}
</style>
