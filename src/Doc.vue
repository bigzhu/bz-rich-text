<template>
  <div>
    <doc :name="name"
      :desc="desc"
      :parm_desc="parm_desc"
      :parms="parms"
      :code="code"
      >
      <bz v-model="rich" @save="call_back"></bz>
    </doc>
  </div>
</template>

<script>
  import Bz from './Bz'
  import Doc from 'bz-doc'
  export default {
    components: {
      Bz,
      Doc
    },
    route: {
      deactivate: function (transition) {
        this.$broadcast('unbind-scroll')
        transition.next()
      }
    },
    data: function () {
      return {
        datas: [1],
        rich: {},
        name: 'bz-rich-text',
        desc: '拉到底部加载',
        parms: [
          {parm: '@call_back', desc: '保存时的触发'}
        ],
        parm_desc: `注意，如果使用的组件有路由，那么最好在切换路由的时候发送消息，解除绑定(参看本例子) <code>this.$broadcast('unbind-scroll')</code>`,
        code: `<bz v-model="rich" @save="call_back"></bz>`
      }
    },
    methods: {
      call_back: function (values) {
        console.log(values)
      }
    }
  }
</script>
