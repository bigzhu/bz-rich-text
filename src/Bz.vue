<template>
  <form v-on:submit.prevent="save" class="ui form">
    <div class="field">
      <label>标题图片</label>
      <bz-upload-picture :img_src="title_img" @upload_done="uploadPicture"></bz-upload-picture>
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
  import BzUploadPicture from 'bz-upload-picture'
  import BzSimditor from 'bz-simditor'
  import upload_picture from './assets/upload-picture.svg'

  export default {
    props: {
      title_img: {
        type: String, default: upload_picture
      },
      title: {
        type: String, default: ''
      },
      summary: {
        type: String, default: ''
      },
      text: {
        type: String, default: ''
      },
      key: {
        type: String, required: true, default: ''
      },
      id: {
        type: Number, default: null
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
        console.log(file_url)
        this.title_img = file_url
      },
      save: function () {
        if (this.title === '') { throw new Error('请填入标题!') }
        if (this.content === '') { throw new Error('请填入内容!') }

        var parm = {
          id: this.id,
          title: this.title,
          title_img: this.title_img,
          summary: this.summary,
          text: this.text,
          key: this.key
        }

        return window.fetch('/api_rich_text', {
          credentials: 'same-origin',
          method: 'post',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(parm)
        })
        .then(function (response) {
          return response.json()
        }).then(function (data) {
          if (data.error !== '0') {
            throw new Error(data.error)
          }
          this.$emit('saved', {title_img: this.title_img, title: this.title, summary: this.summary, text: this.text, key: this.key})
        })
      }
    }
  }
</script>
