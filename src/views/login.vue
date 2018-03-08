<template>
  <div>
    <div class="login-wrap" v-show="showLogin">
      <h3>登陆</h3>
      <p v-show="showPrompt">{{prompt}}</p>
      <input type="text" placeholder="用户名" v-model="username">
      <input type="password" placeholder="密码" v-model="password">
      <button @click="login">登陆</button>
      <span @click="toRegister">没有账号？马上注册</span>
    </div>

    <div class="register-wrap" v-show="showRegister">
      <h3>注册</h3>
      <p v-show="showPrompt">{{prompt}}</p>
      <input type="text" placeholder="请输入用户名" v-model="newUsername">
      <input type="password" placeholder="请输入密码" v-model="newPassword">
      <button @click="register">注册</button>
      <span @click="toLogin">已有账号？马上登陆</span>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {setCookie, getCookie} from '../assets/cookie'
  export default {
    data(){
      return {
        showLogin: true,
        showRegister: false,
        showPrompt: false,
        prompt: '',
        username: '',
        password: '',
        newUsername: '',
        newPassword: ''
      }
    },
    mounted(){
      if (getCookie('username')) {
        this.$router.push('/home')
      }
    },
    methods:{
        login(){
            if(this.username == '' || this.password == ''){
                alert("请输入用户名或密码")
            }else{
                let data = {
                    'username':this.username,
                  'password':this.password
                }
                //接口请求
              this.$http.post('http://localhost/vueapi/index.php/Home/user/login',data).then((res) => {
                    console.log(res);
                    if(res.data == -1){
                        this.prompt = "该用户名不存在"
                      this.showPrompt = true
                    }else if(res.data == 0){
                        this.prompt = "密码输入有误";
                        this.showPrompt = true
                    }else if(this.data == "admin"){
                        this.$router.push('/admin')
                    }else{
                        this.prompt = "登陆成功"
                      this.showPrompt = true;
                        setCookie('username',this.username,1000*60);
                        setTimeout(function () {
                          this.$router.push('/home')
                        }.bind(this),1000)
                    }
              })
            }
        },
      register(){
            if(this.newUsername == ''|| this.password == ''){
                alert("请输入用户名和密码")
            }else{
                let data = {
                    'username':this.username,
                  'password':this.password
                }
                this.$http.post('http://localhost/vueapi/index.php/Home/user/register',data).then((res)=>{
                    console.log(res);
                    if(res.data == 'ok'){
                        this.prompt = "注册成功"
                      this.showPrompt = true
                      this.username = '';
                        this.password = ''
                      //注册成功再跳回登录页
                      setTimeout(function () {
                        this.showRegister = false
                        this.showLogin = true
                        this.showPrompt = false
                      }.bind(this),1000)
                    }
                });
            }
      },
      toRegister(){
            this.showRegister = true
        this.showLogin = false
      },
      toLogin(){
          this.showRegister = false
        this.showLogin = true
      }
    }
  }
</script>
<style>
  .login-wrap {
    text-align: center;
  }

  input {
    display: block;
    width: 250px;
    height: 40px;
    margin: 10px auto;
    line-height: 40px;
    outline: none;
    box-sizing: border-box;
    border: 1px solid #888;
    padding: 10px;
  }

  p {
    color: red;
  }

  button {
    display: block;
    width: 250px;
    height: 40px;
    margin: 10px auto;
    line-height: 40px;
    outline: none;
    box-sizing: border-box;
    border: none;
    background-color: #41b883;
    color: #fff;
    font-size: 16px;
  }

  span {
    cursor: pointer;
  }

  span:hover {
    color: #41b883;
  }
</style>
