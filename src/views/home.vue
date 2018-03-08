<template>
  <div>
    <h3>欢迎{{name}}</h3>
    <a href="javascript:void(0)" @click="quit">注册登陆</a>
  </div>
</template>
<script type="text/javascript">
  import {setCookie, getCookie, delCookie} from '../assets/cookie'
  export default{
    data(){
      return {
        name: ''
      }
    },
    mounted(){
      //页面挂载获取保存的cookie值，渲染到页面上
      let uname = getCookie('username')
      this.name = uname;
      //如果cookie不存在，则跳转到登录页
      if (uname = '') {
        this.$router.push('/');
      }
    },
    methods: {
      quit(){
        delCookie('username')
        this.$router.push('/')
      }
    }
  }
</script>
<style>

</style>
