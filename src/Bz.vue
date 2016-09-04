<style lang=less>
</style>

<template>
<form v-on:submit.prevent="save" class="ui form">
  <div class="field">
    <label>标题图片</label>
    <bz-upload-picture :call_back="uploadPicture"></bz-upload-picture>
  </div>
  <div class="required field">
    <label>标题</label>
    <input v-model="title" type="text" name="title" placeholder="标题">
  </div>
  <div class="field">
    <label>摘要</label>
    <bz-simditor :content.sync="summary"></bz-simditor>
  </div>
  <div class="required field">
    <label>内容</label>
    <bz-simditor :content.sync="text"></bz-simditor>
  </div>
  <button class="ui button" type="submit">保存</button>
</form>
</template>

<script>
  import 'bz-semantic-ui-form'
  import BzUploadPicture from 'bz-upload-picture'
  import BzSimditor from 'bz-simditor'

  import Vue from 'vue'
  import VueResource from 'vue-resource'
  Vue.use(VueResource)
  var api_rich_text = Vue.resource('/api_rich_text{/parm}')

  export default {
    props: {
      title_img: {
        type: String,
        twoWay: true,
        default: ''
      },
      title: {
        type: String,
        twoWay: true,
        default: ''
      },
      summary: {
        type: String,
        twoWay: true,
        default: ''
      },
      text: {
        type: String,
        twoWay: true,
        default: ''
      },
      key: {
        type: String,
        default: ''
      },
      save_call_back: {
        type: Function,
        required: true
      }
    },
    components: {
      BzUploadPicture,
      BzSimditor
    },
    data: function () {
      return {
      }
    },
    ready () {
    },
    methods: {
      uploadPicture: function (file_url) {
        this.title_img = file_url
      },
      save: function () {
        if (this.title === '') throw new Error('请填入标题!')
        if (this.content === '') throw new Error('请填入内容!')

        var parm = JSON.stringify(
          {
            title: this.title,
            title_img: this.title_img,
            summary: this.summary,
            text: this.text,
            key: this.key
          }
        )
        api_rich_text.save(parm).then(
          (response) => {
            if (response.data.error !== '0') throw new Error(response.data.error)
            if (this.save_call_back) this.save_call_back(this.title_img, this.title, this.summary, this.text, this.key)
          },
          (response) => {
            throw new Error(response.statusText)
          }
        )
      }
    }
  }
</script>
