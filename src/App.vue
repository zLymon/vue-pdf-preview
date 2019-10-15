<template>
  <div id="app">
    <div id="main">
      <div class="toolbar">
        <div class="page-control">
          <button :disabled="prevDisable" @click="prevPage">
            <svg t="1570849872944" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2327" width="20" height="20"><path d="M472.064 272.448l-399.232 399.232c-22.08 22.08-22.08 57.792 0 79.872 22.016 22.016 57.792 22.08 79.872 0L512 392.256l359.296 359.296c22.016 22.016 57.792 22.08 79.872 0 22.08-22.08 22.016-57.792 0-79.872L551.936 272.448C529.856 250.432 494.144 250.432 472.064 272.448z" p-id="2328" fill="#ffffff"></path></svg>
          </button>
          <button :disabled="nextDisable" @click="nextPage">
            <svg t="1570849885759" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2480" width="20" height="20"><path d="M472.064 751.552 72.832 352.32c-22.08-22.08-22.08-57.792 0-79.872 22.016-22.016 57.792-22.08 79.872 0L512 631.744l359.296-359.296c22.016-22.016 57.792-22.08 79.872 0 22.08 22.08 22.016 57.792 0 79.872l-399.232 399.232C529.856 773.568 494.144 773.568 472.064 751.552z" p-id="2481" fill="#ffffff"></path></svg>
          </button>
          <input
            type="text"
            v-model="currentPage"
            @keyup.enter="changePage">
          <span> / </span>
          <span>{{ pdfData.numPages }}</span>
        </div>
        <div class="size-control">
          <button @click="resize('add')">+</button>
          <button @click="resize('minus')">-</button>
          <p>当前放大比例：{{ scale | toFixed }}</p>
        </div>
        <div class="upload">
          <a
            href="javascript:;"
            class="file-wrapper"
            @click="isUrlInputShow = true">
            输入路径
          </a>
          <a
            href="javascript:;"
            class="file-wrapper">
            选择文件
            <input
              ref="pdfFile"
              type="file"
              accept="application/pdf"
              @change="handleUpload">
          </a>
          <span v-if="fileName !== ''">当前文件(本地上传)：{{ fileName }}</span>
        </div>
        <div
          class="full-screen"
          @click="fullscreen">
          <svg t="1570849669055" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1120" width="20" height="20"><path d="M218.688721 168.644955l-0.013303 0 0-0.014326L218.688721 168.644955l128.45665-0.556679c19.003819 0.508583 36.624129-13.658068 39.493479-37.807071L386.638851 104.19713c-0.474814-20.503986-16.169261-37.442774-35.158754-37.897122l-245.311055 1.561565c-0.37146-0.040932-0.605797-0.186242-0.935302-0.186242l-17.193591-0.412392c-9.494235-0.24764-18.012236 3.756557-24.087605 10.389628-6.151094 6.591115-8.428973 15.797801-8.18031 26.090215l1.843998 18.562774c0 0.37146-1.280156 0.61296-1.280156 1.025353l0.956791 265.449721c0.412392 20.454868 16.121166 32.736589 35.165918 33.190937l25.552979 0c19.003819 0.454348 34.092469-17.359366 33.665751-37.786605l-1.073448-144.370085 169.152515 179.211615c18.377556 19.84293 48.128137 19.84293 66.518996 0 18.329461-19.835767 18.329461-51.897997 0-71.755254L218.688721 168.644955z" p-id="1121" fill="#ffffff"></path><path d="M948.293761 899.895471c-0.034792-0.460488 1.238201-0.722454 1.238201-1.073448l-0.014326-0.007163-0.977257-261.362636c-0.481977-20.441565-16.182564-32.67519-35.186384-33.128515l-24.163329-0.034792c-18.934235-0.467651-34.085306 17.385972-33.603329 37.849026l1.073448 141.253094L687.535899 603.016947c-18.405185-19.850094-48.106648-19.850094-66.49853 0-18.36323 19.835767-18.36323 51.897997 0 71.734788l169.104419 180.25027-128.297015 0.550539c-18.989493-0.454348-36.576033 12.115945-39.493479 36.273135l0 26.083052c0.481977 20.4958 16.237823 37.442774 35.17922 37.951357l242.160295-1.582031c0.391926 0 0.611937 0.164752 0.970094 0.164752l17.173125 0.454348c9.495258 0.240477 18.054191-3.76372 24.115234-10.417257 6.212492-6.529717 8.407484-15.763009 8.222265-26.062586L948.293761 899.895471z" p-id="1122" fill="#ffffff"></path><path d="M386.225435 603.009783l-0.014326 0c-18.36323-19.84907-48.14144-19.84907-66.505693 0L150.561088 782.214235l1.086751-142.119835c0.413416-20.4958-14.675234-38.364773-33.610492-37.849026l-25.574469 0c-19.051915 0.509606-34.745339 12.68695-35.172057 33.183773l-0.99056 263.323291c0 0.37146 1.286296 0.61296 1.286296 1.025353l-1.843998 18.521842c-0.206708 10.299577 2.00875 19.540032 8.167007 26.062586 6.068206 6.653537 14.634302 10.657734 24.067138 10.416234l17.207917-0.454348c0.358157 0 0.578168-0.164752 0.970094-0.164752l245.290589 1.589195c18.989493-0.522909 34.71157-17.455557 35.172057-37.95852l0-26.076912c-2.862187-24.157189-20.482497-36.72032-39.438221-36.273135l-129.955794-0.454348 169.001065-180.24413C404.568199 654.970202 404.568199 622.845551 386.225435 603.009783z" p-id="1123" fill="#ffffff"></path><path d="M941.984055 77.652568c-6.068206-6.632047-14.634302-10.637268-24.115234-10.354835l-17.152658 0.412392c-0.385786 0-0.61296 0.144286-0.99056 0.206708L654.063553 66.333778c-19.017122 0.474814-34.704406 17.414625-35.186384 37.910425l-0.048095 26.090215c2.917446 24.190959 20.565385 38.302351 39.486316 37.841863l130.368186 0.556679L621.181655 347.381755c-18.418488 19.75288-18.418488 51.815109-0.048095 71.6519l0 0.007163c18.377556 19.84293 48.093345 19.84293 66.491367 0l169.132048-180.327018-1.086751 145.471163c-0.461511 20.434401 14.696724 38.309514 33.617655 37.85619l24.163329 0c18.989493-0.522909 34.69008-12.756535 35.172057-33.197076l0.977257-265.512143c0-0.412392-1.258667-0.661056-1.258667-1.032516l1.851161-18.562774C950.391539 93.449345 948.183244 84.237543 941.984055 77.652568z" p-id="1124" fill="#ffffff"></path></svg>
        </div>
      </div>
      <div class="url-input" v-show="isUrlInputShow">
        <input type="text" v-model="url">
        <button @click="submit">确定</button>
      </div>
      <div id="container"></div>
    </div>
  </div>
</template>

<script>
import PDFJS from 'pdfjs-dist'
import { TextLayerBuilder } from 'pdfjs-dist/web/pdf_viewer'
// 用于渲染文本的样式和canvas上的字体一一对应
import 'pdfjs-dist/web/pdf_viewer.css'
// 某些情况下无法读取到本地的pdf.worker.js，若无法读取该文件则使用cdn里的pdf.worker.js，版本需要对应上
PDFJS.GlobalWorkerOptions.workerSrc =
  '//cdnjs.cloudflare.com/ajax/libs/pdf.js/2.0.943/pdf.worker.js'
export default {
  name: 'app',
  data () {
    return {
      pdfData: '',
      url: '',
      pdfText: {
        items: []
      },
      scale: 1,
      currentPage: 1,
      isFullScreen: false,
      fileName: '',
      isUrlInputShow: false
    }
  },
  filters: {
    toFixed (num) {
      return num.toFixed(1)
    }
  },
  computed: {
    nextDisable () {
      if (this.currentPage === this.pdfData.numPages) {
        return true
      }
      return false
    },
    prevDisable () {
      if (this.currentPage === 1) {
        return true
      }
      return false
    }
  },
  mounted () {
    if (this.url) {
      this.getPDF()
    }
    const that = this
    document.onkeydown = function (e) {
      // 取消全屏
      if (that.isFullScreen) {
        if (window.event.keyCode === 27) {
          that.isFullScreen = false
          return
        }

        if (window.event.keyCode === 37 || window.event.keyCode === 38) {
          e.preventDefault()
          if (!that.checkPageValid('minus')) return
          that.currentPage--
          that.changePage()
          that.fullscreen()
          return
        }

        if (window.event.keyCode === 39 || window.event.keyCode === 40) {
          e.preventDefault()
          if (!that.checkPageValid('add')) return
          that.currentPage++
          that.changePage()
          that.fullscreen()
        }
      }
    }
  },
  methods: {
    async getPDF (fileData = null) {
      if (!fileData) {
        const pdf = await PDFJS.getDocument(this.url)
        this.pdfData = pdf
        // pdf数据页数从1开始
        for (let i = 1; i <= pdf.numPages; i++) {
          try {
            this.renderPdf(pdf, i)
          } catch (e) {
            console.error(e)
          }
        }
      } else {
        const pdf = await PDFJS.getDocument(fileData)
        this.pdfData = pdf
        // pdf数据页数从1开始
        for (let i = 1; i <= pdf.numPages; i++) {
          try {
            this.renderPdf(pdf, i)
          } catch (e) {
            console.error(e)
          }
        }
      }
    },
    reRenderPdf () {
      for (let i = 1; i <= this.pdfData.numPages; i++) {
        try {
          this.renderPdf(this.pdfData, i)
        } catch (e) {
          console.error(e)
        }
      }
    },
    async renderPdf (pdf, num) {
      const page = await pdf.getPage(num)
      const viewport = page.getViewport(this.scale)

      // 存放canvas的元素
      const container = document.getElementById('container')
      const pageDiv = document.createElement('div')

      const canvas = document.createElement('canvas')
      const context = canvas.getContext('2d')

      pageDiv.setAttribute('id', `page-${page.pageIndex + 1}`)
      pageDiv.setAttribute('style',
        `
          position: relative;
          display: flex;
          justify-content: center;
          padding-top: 50px`)
      pageDiv.appendChild(canvas)

      canvas.height = viewport.height
      canvas.width = viewport.width

      const renderContext = {
        canvasContext: context,
        viewport
      }

      page.render(renderContext)
      container.appendChild(pageDiv)
      this.renderText(page, viewport, pageDiv)
    },
    async renderText (page, viewport, pageDiv) {
      const textContent = await page.getTextContent()
      const textLayerDiv = document.createElement('div')

      this.pdfText.items = this.pdfText.items.concat(textContent.items)

      // 用于文字层与canvas层文字对齐
      textLayerDiv.setAttribute('class', 'textLayer')
      textLayerDiv.setAttribute(
        'style',
        `width: ${viewport.width}px; margin: 0 auto; margin-top: 50px`
      )

      const textLayer = new TextLayerBuilder({
        textLayerDiv: textLayerDiv,
        pageIndex: page.pageIndex,
        viewport
      })

      textLayer.setTextContent(textContent)
      textLayer.render()
      pageDiv.appendChild(textLayerDiv)
    },
    resize (type) {
      type === 'add'
        ? this.scale += 0.1
        : this.scale -= 0.1
      document.getElementById('container').innerHTML = ''
      this.$nextTick(() => {
        this.reRenderPdf()
      })
    },
    nextPage () {
      if (!this.checkPageValid('add')) return

      this.currentPage++
      this.changePage()
    },
    prevPage () {
      if (!this.checkPageValid('minus')) return
      this.currentPage--
      this.changePage()
    },
    checkPageValid (type) {
      if (this.currentPage > 1 && this.currentPage < this.pdfData.numPages) {
        return true
      }
      if (type === 'minus' && this.currentPage === 1) {
        alert('已经是第一页了')
        return false
      }
      if (type === 'add' && this.currentPage === this.pdfData.numPages) {
        alert('已经是最后一页了')
        return false
      }
      return true
    },
    changePage () {
      location.hash = `#page-${this.currentPage}`
    },
    fullscreen () {
      this.isFullScreen = true
      document.getElementById(`page-${this.currentPage}`).requestFullscreen()
    },
    handleUpload (e) {
      const file = this.$refs.pdfFile.files[0]
      this.fileName = file.name
      const reader = new FileReader()
      reader.readAsDataURL(file, 'UTF-8')
      reader.onload = evt => {
        const fileString = evt.target.result
        document.getElementById('container').innerHTML = ''
        this.getPDF(fileString)
      }
    },
    submit () {
      document.getElementById('container').innerHTML = ''
      this.getPDF()
      this.isUrlInputShow = false
    }
  }
}
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
}
#app {
  height: 100%;
}
#main {
  width: 100%;
  overflow: auto;
  background-color: #3e3e3e;
  height: 100%;
}
.toolbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #fff;
  height: 50px;
  padding: 0 20px;
  background-color: #474747;
  z-index: 9999;
}
.toolbar button {
  width: 30px;
  height: 30px;
  background-color: transparent;
  font-size: 18px;
  padding: 0;
  color: #FFF;
  border: none;
  outline: none;
  margin-right: 10px;
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
  border-right-color: transparent;
  cursor: pointer;
}
.toolbar button:disabled:hover {
  border: none;
  cursor: not-allowed;
}
.toolbar button:hover {
  border: 1px solid #fff;
}
.toolbar p {
  display: inline-block;
  color: #fff;
  font-size: 18px;
}
.upload {
  display: flex;
  align-items: center;
}
.toolbar .file-wrapper {
  position: relative;
  display: inline-block;
  background: #D0EEFF;
  border: 1px solid #99D3F5;
  border-radius: 4px;
  padding: 5px;
  font-size: 14px;
  overflow: hidden;
  color: #1E88C7;
  text-decoration: none;
  text-indent: 0;
  cursor: pointer;
}
.file-wrapper {
  margin-right: 10px
}
.file-wrapper:last-child {
  margin-right: 0;
}
.file-wrapper input {
  position: absolute;
  font-size: 100px;
  right: 0;
  top: 0;
  opacity: 0;
  cursor: pointer;
}
.file-wrapper:hover {
  background: #AADFFD;
  border-color: #78C3F3;
  color: #004974;
  text-decoration: none;
}
.page-control {
  display: inline-block;
  color: #FFF;
  font-size: 18px;
}
.page-control input {
  width: 40px;
  height: 30px;
  background-color: #5e5e5e;
  text-align: center;
  font-size: 18px;
  color: #FFF;
  border: rgba(0, 0, 0, .42);
}
.fullscreen {
  cursor: pointer;
}
.url-input {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 50%;
  left: 50%;
  right: 0;
  bottom: 0;
  width: 200px;
  height: 100px;
  border: 1px solid #b9b9b9;
  border-radius: 10px;
  transform: translate(-50%, -50%);
  background-color: #b9b9b9;
  z-index: 9999;
}
</style>
