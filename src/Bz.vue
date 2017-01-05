<template>
  <form v-on:submit.prevent="save" class="ui form">
    <div class="field">
      <label>标题图片</label>
      <bz-upload-picture :img_src="value.title_img" @upload_done="uploadPicture"></bz-upload-picture>
    </div>
    <div class="required field">
      <label>标题</label>
      <input v-model="value.title" type="text" name="title" placeholder="标题">
    </div>
    <div class="field">
      <label>摘要</label>
      <bz-simditor v-model="value.summary"></bz-simditor>
    </div>
    <div class="required field">
      <label>内容</label>
      <bz-simditor v-model="value.text"></bz-simditor>
    </div>
    <div class="required field">
      <label>key</label>
      <input v-model="value.key" type="text" name="title" placeholder="key">
    </div>
    <button class="ui button" type="submit">保存</button>
  </form>
</template>

<script>
  import BzUploadPicture from 'bz-upload-picture'
  import BzSimditor from 'bz-simditor'

  export default {
    props: {
      value: {
        requried: true,
        default: function () { return {} }
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
    methods: {
      uploadPicture: function (file_url) {
        this.value.title_img = file_url
      },
      save: function () {
        let self = this
        let method = 'post'
        if (this.value.id) { // 如果有id,就put来修改
          method = 'put'
        }

        let parm = this.value

        return window.fetch('/api_rich_text', {
          credentials: 'same-origin',
          method: method,
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
          self.$emit('save', self.value)
          self.$emit('input', self.value)
        })
      }
    }
  }
</script>
