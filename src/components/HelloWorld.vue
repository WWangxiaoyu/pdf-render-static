<template>
  <div class="HelloWorld">
    <div style="margin-top: 10px">
      <p>预览默认pdf</p>
      <button @click="checkLocal">打开</button>
    </div>

    <div style="margin-top: 40px">
      <input type="file" name="myfile" id="myfile" @change="preview($event)"/>
      <iframe v-if="showPdf" id='previewPdf' :src="'/static/pdfjs/web/viewer.html?file=' + fileUrl" 
              height="560" width="100%">
      </iframe>
      <p>当前pdf总页数为{{ totalPageNum }}</p>
    </div>
  </div>

</template>

<script>
import PDFJS from 'pdfjs-dist/legacy/build/pdf'
export default {
  name: 'HelloWorld',
  data () {
    return {
      fileUrl: '',
      showPdf: false,
      contractVisable: false,
      totalPageNum: 0 
    }
  },
  methods: {
    checkLocal() {
      let url = '2022创造营学员手册.pdf'
      window.open('/static/pdfjs/web/viewer.html?file=' + url)
    },
    preview(event) {
      //document.getElementById('file')获得id名为myfile的dom对象
      //files属性返回一个FileList对象，表示使用文件上载按钮选择的文件。
      //通过FileList对象，可以获取文件的名称，大小和内容，此属性是只读的。
      let files = document.getElementById('myfile').files[0]
      this.showPdf = true
      this.fileUrl = this.getObjectURL(files)
      // 获取pdf总页数
      const loadingTask = PDFJS.getDocument(this.fileUrl)
      loadingTask.promise.then(pdf => {
        this.totalPageNum = pdf.numPages
      })
    },
    getObjectURL(file) {
      let url = null;
      if (window.createObjectURL != undefined) { // basic
          url = window.createObjectURL(file);
      } else if (window.webkitURL != undefined) { // webkit or chrome
          url = window.webkitURL.createObjectURL(file);
      } else if (window.URL != undefined) { // mozilla(firefox)
          url = window.URL.createObjectURL(file);
      }
      return url;
    },
  }
}
</script>

<style scoped>

</style>
