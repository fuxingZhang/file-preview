<template>
  <div class="preview">
    <div v-if="isEmpty">文件不存在</div>
    <div v-else class="content">
      <iframe
        v-if="type=='.pdf'"
        :src="'/pdfjs/web/viewer.html?file='+file"
        width="100%"
        height="100%"
        scrolling="no"
      >您的浏览器不支持PDF阅读</iframe>
      <iframe
        v-else
        :src="'https://view.officeapps.live.com/op/view.aspx?src='"
        width="100%"
        height="100%"
        frameborder="1"
      ></iframe>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData ({ params }) {
    // { 
    //   code: 200,
    //   data: {
    //     created: "2019-05-24 08:13:23"
    //     end_time: "20190527161323739"
    //     file: "http://localhost:3001/20190524161323739.docx"
    //     id: 6
    //     name: "20190524161323739"
    //     type: ".docx"
    //   }
    // }
    let { data } = await axios.get(`/preview`,{
      params:{
        id:params.id
      }
    })
    console.log(data)
    const code=data.code
    if (code==400) {
      return {isEmpty:true}
    }else{
      const type=data.data.type  // .docx
      const file=data.data.file  //http://localhost:3001/20190524161323739.docx
      console.log(type)
      console.log(file)
      return {isEmpty:false,type,file}
    }
  }
}
</script>

<style scoped>
.preview,
.content {
  height: 100%;
  width: 100%;
}
</style>


