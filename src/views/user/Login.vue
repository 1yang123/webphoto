<!--denglu-->
<template>
  <div class="bg">
    <div class="z">
      <div class="q">
        ■■■
      </div>
      <div class="wz">
        零点云相册 随意上传 无限空间
      </div>
      <div class="wz1">
        智能识别整理 为您贴心管理相册
      </div>
      <div class="q1">
        ■■■
      </div>
      <div class="tplb">
        <div class="tp">
          <img src="../../assets/tubiao/1tb.png">
        </div>
        <div class="tp">
          大图预览
        </div>
      </div>
      <div class="tplb">
        <div class="tp">
          <img src="../../assets/tubiao/2tb.png">
        </div>
        <div class="tp">
          原图保存
        </div>
      </div>
      <div class="tplb">
        <div class="tp">
          <img src="../../assets/tubiao/3tb.png">
        </div>
        <div class="tp">
          下载方便
        </div>
      </div>
    </div>
    <div class="login-box1">

      <el-form ref="form" :model="form" :rules="rules" label-width="80px;" class="login-box">
        <div class="login-title">登录</div>
        <!--  用户名-->
        <el-form-item label=" " prop="username"><h2>Username</h2>
          <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
        </el-form-item>
        <!--      密码-->
        <div class="login">
          <el-form-item label=" " prop="password"><h2>Password</h2><img @click="trans()" :src="imgUrl" alt="" id="test">
            <!--<el-form-item label=" " prop="password"><h2>密码</h2><img id="test" src="../../assets/tubiao/test_1.png" alt="">-->
            <el-input type="password" id="psw" placeholder="请输入密码" v-model="form.password"/>
          </el-form-item>
        </div>
        <el-form-item class="login-button">
          <!--<el-button type="primary" plain @click="submitForm('form')">登录</el-button>-->
        <button @click="submitForm('form')" type="button" class="styled-button"> <span class="styled-button__real-text-holder"> <span
          class="styled-button__real-text">login</span> <span class="styled-button__moving-block face"> <span
          class="styled-button__text-holder"> <span class="styled-button__text">login</span> </span> </span><span
          class="styled-button__moving-block back"> <span class="styled-button__text-holder"> <span
          class="styled-button__text">login</span> </span> </span> </span> </button>
          <div class="zc">
            <div class="wzw">没有账号？点击注册-></div>
            <el-button type="info" plain @click="gotolink()">register</el-button>
          </div>
        </el-form-item>
      </el-form>
    </div>


  </div>
</template>

<script>
export default {
  name: "Login",
  data: function () {
    return {
      picVisble:false,
      imgArr:['test_1.png','test.png'],
      imgIndex:0,

      vedioCanPlay: false,
      fixStyle: '',
      form: {
        username: '',
        password: '',
        rememberMe: false,
        code: '',
        uuid: '',
      },
      rules: {
        username: [
          {required: true, message: '请输入账号', trigger: 'blur'},
          {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur'}
        ],
      }
    }
  },
 computed:{
    imgUrl:function () {
      return require("../../assets/tubiao/"+this.imgArr[this.imgIndex]);
    }
 },
  // watch(){
  //   let test = document.getElementById('test');
  //   let psw = document.getElementById('psw');
  //   let flag = 0;
  //   test.onclick = function () {
  //     if (flag === 0) {
  //       psw.type = 'text';
  //       test.src = '../../assets/tubiao/test.png';
  //       flag = 1
  //     } else {
  //       psw.type = 'password';
  //       test.src = '../../assets/tubiao/test_1.png';
  //       flag = 0;
  //     }
  //   }
  // },
  methods: {
    trans(){
      let test = document.getElementById('test');
      let psw = document.getElementById('psw');
        if (psw.type === 'password') {
          psw.type = 'text';
        }else{
          psw.type = 'password';
        }
        this.imgIndex--;
        if(this.imgIndex < 0){
          this.imgIndex = this.imgArr.length - 1;
        }
    },
    gotolast(){
      // 点击跳转至上次浏览页面
      this.$router.go(-1);
    },
    gotolink(){
      //指定跳转地址
      this.$router.replace('/register');
    },
    submitForm: function (formName) {
      this.$refs[formName].validate(async valid => {
        if (valid) {
          //测试数据交互
          this.$http.post("http://localhost:8086/login",{
            username: this.form.username,
            password: this.form.password
          })
            .then(res => {
              if(res.data === 'success'){

                // 登录成功
                console.log("登录成功！");
                console.log(res.data);
                /** 将Token保存到localStorage*/
                const authorization = res.data.Authorization;
                localStorage.token = authorization;
                this.msg = authorization;
                //是否登录
                sessionStorage.setItem('isLogin', 'true');
                this.$store.dispatch('asyncUpdateUser', {username: this.form.username});
                // alert('submit!');1
                this.$message({
                  message: '恭喜你，验证通过！',
                  duration: 500,
                  type: 'success'
                });
                //编程式导航，以代码方式跳转
                // this.$router.push("/main");
                this.$router.push({name: 'Home1', params: {username: this.form.username}});
              }
              else {
                this.$message({
                  message: '账号或密码错误！',
                  duration: 500,
                  type: 'warning'
                });
                return false;
              }
            })
            .catch(error => {
              this.$message({
                message: '账号或密码错误！',
                duration: 500,
                type: 'warning'
              });
              return false;
            });
        }
      });
    }
  },

}

</script>

<style scoped>
  .z{
    display: inline-block;
    width: 34%;
    padding-top: 200px;
    padding-left: 180px;
    text-align: left;
  }
  .q{
    color: rgba(255, 255, 255, 0.78);
    font-size: 30px;
  }
  .q1{
    color: rgba(255, 255, 255, 0.78);
    font-size: 30px;
    float: right;
  }
  .wz{
    color: whitesmoke;
    font-size: 40px;
    margin-left: 30px;
    margin-bottom: 10px;
    margin-top: 30px;
  }
  .wz1{
    color: #ffffff;
    font-size: 26px;
    margin-left: 30px;
    line-height: 60px;
  }
  .bg{
    width: 100%;
    height: 100%;
    position:fixed;
    min-width: 1000px;
    z-index:-10;
    zoom: 1;
    background-image: url("../../assets/bj/bjbjbjbjbj.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    -webkit-background-size: cover;
    -o-background-size: cover;
    background-position: center 0;
  }

  .login-box1{
    color: #000000;
    width:50%;
    margin-top: 20px;
    float: right;
    display: inline-block;
    align-content: center;
    position: fixed;
  }
  .login-box{
    position: relative;
    width: 450px;
    margin:170px auto;
    border: 1px solid #DCDFE6;
    padding: 35px;
    border-radius: 5px;
    box-shadow: 0 0 45px #9899a0;
    background-color: rgba(255, 255, 255, 0.88);

  }
  .login-title{
    text-align: center;
    font-size: 20px;
  }
  .login-button{
    margin:50px 10px 30px;
  }
  .wzw{
    color: #000000;
    display: inline-block;
  }
  .zc{
    display: inline-block;
    float: right;
  }
  .tp{
    font-size: 18px;
    color: white;
    margin: 20px;
    text-align: center;
    float: left;
    width: 100px;
    display: inline-block;
  }
  img{
    width: 56%;
  }
  .tplb{
    margin-left: 40px;
    margin-top: 100px;
    width: 120px;
    float: left;
    display: inline-block;
  }

  /*密码*/
  .login {
    position: relative;
  }
  .login img{
    position: absolute;
    top: 5px;
    right: 0px;
    width: 20px;
    height: 20px;
  }
  /*yangshi*/
  main {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    background: #3f2766;
  }
  .form {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    position: relative;
    width: 400px;
    height: 400px;
    -ms-flex-negative: 0;
    flex-shrink: 0;
    padding: 20px;
    border-radius: 5px;
  }
  .form__loader {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    z-index: -4;
    -webkit-transition: all 0.5s ease;
    transition: all 0.5s ease;
  }
  .form__content {
    text-align: center;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;
    position: relative;
    opacity: 0;
    -webkit-transform: translateY(10px);
    transform: translateY(10px);
    -webkit-transition: all 0.5s ease 0.7s;
    transition: all 0.5s ease 0.7s;
  }
  .form__cover {
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    z-index: -4;
    border-radius: 7px;
    overflow: hidden;
    -webkit-transition: all 0.3s ease 0.8s;
    transition: all 0.3s ease 0.8s;
    box-shadow: 0 0 0 0 rgba(0, 0, 0, 0);
  }
  .form__cover:after {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    background: #4d317a;
    z-index: -4;
    border-radius: 50%;
    -webkit-transition: all 1.5s ease 0.3s;
    transition: all 1.5s ease 0.3s;
    -webkit-transform: scale(0);
    transform: scale(0);
  }
  .form__cover:before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    background: white;
    z-index: -5;
    border-radius: 50%;
    -webkit-transition: all 0.5s ease;
    transition: all 0.5s ease;
    -webkit-transform: scale(0);
    transform: scale(0);
  }
  body.on-start .form__cover:before {
    -webkit-transform: scale(0.15);
    transform: scale(0.15);
  }
  body.document-loaded .form__loader {
    -webkit-transform: scale(0);
    transform: scale(0);
    opacity: 0;
    visibility: hidden;
  }
  body.document-loaded .form__content {
    opacity: 1;
    -webkit-transform: none;
    transform: none;
  }
  body.document-loaded .form__cover {
    box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
  }
  body.document-loaded .form__cover:after {
    -webkit-transform: scale(2);
    transform: scale(2);
  }
  body.document-loaded .form__cover:before {
    -webkit-transition: opacity 0.3s ease 0.8s, -webkit-transform 2s ease;
    transition: opacity 0.3s ease 0.8s, -webkit-transform 2s ease;
    transition: transform 2s ease, opacity 0.3s ease 0.8s;
    transition: transform 2s ease, opacity 0.3s ease 0.8s, -webkit-transform 2s ease;
    -webkit-transform: scale(2);
    transform: scale(2);
    opacity: 0;
  }
  h1 {
    font-size: 40px;
    margin: 15px 0 20px 0;
    letter-spacing: 0.05em;
    color: #714cab;
    font-weight: 700;
  }
  /*按钮*/
  .styled-button {
    -webkit-appearance: none;
    -webkit-user-select: none;
    cursor: pointer;
    font-size: 14px;
    width: 100%;
    padding: 20px;
    outline: none;
    background: none;
    position: relative;
    color: #a0a0a0;
    border-radius: 3px;
    margin-bottom: 25px;
    border: none;
    text-transform: uppercase;
    font-weight: 700;
    letter-spacing: 0.1em;
    background: #505050;
    -webkit-transition: all 0.3s ease;
    transition: all 0.3s ease;
    overflow: hidden;
  }
  .styled-button__real-text-holder {
    position: relative;
  }
  .styled-button__real-text {
    color: transparent;
    display: inline-block;
  }
  .styled-button__text-holder {
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-transition: all 0.3s ease;
    transition: all 0.3s ease;
  }
  .styled-button__moving-block {
    -webkit-transition: all 0.3s ease;
    transition: all 0.3s ease;
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 100%;
    overflow: hidden;
  }
  .styled-button__moving-block.back {
    color: white;
    -webkit-transform: translateX(-100%);
    transform: translateX(-100%);
  }
  .styled-button__moving-block.back .styled-button__text-holder {
    -webkit-transform: translateX(100%);
    transform: translateX(100%);
  }
  .styled-button:hover,
  .styled-button:active {
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.6);
    background: #000000;
  }
  .styled-button:hover .face,
  .styled-button:active .face {
    -webkit-transform: translateX(100%);
    transform: translateX(100%);
  }
  .styled-button:hover .face .styled-button__text-holder,
  .styled-button:active .face .styled-button__text-holder {
    -webkit-transform: translateX(-100%);
    transform: translateX(-100%);
  }
  .styled-button:hover .back,
  .styled-button:active .back {
    -webkit-transform: translateX(0);
    transform: translateX(0);
  }
  .styled-button:hover .back .styled-button__text-holder,
  .styled-button:active .back .styled-button__text-holder {
    -webkit-transform: translateX(0);
    transform: translateX(0);
  }
  .styled-button:active {
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  }
  .styled-input {
    width: 100%;
    position: relative;
    margin-bottom: 25px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 3px;
    -webkit-transition: all 0.3s ease;
    transition: all 0.3s ease;
  }
  .styled-input__circle {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
    overflow: hidden;
    border-radius: 3px;
  }
  .styled-input__circle:after {
    content: '';
    position: absolute;
    left: 16.5px;
    top: 19px;
    height: 14px;
    width: 14px;
    z-index: -2;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.15);
    box-shadow: 0 0 10px rgba(255, 255, 255, 0);
    -webkit-transition: opacity 1s ease, -webkit-transform 0.6s ease;
    transition: opacity 1s ease, -webkit-transform 0.6s ease;
    /*transition: transform 0.6s ease, opacity 1s ease;*/
    transition: transform 0.6s ease, opacity 1s ease, -webkit-transform 0.6s ease;
  }
  .styled-input__input {
    width: 100%;
    -webkit-appearance: none;
    font-size: 14px;
    outline: none;
    background: none;
    padding: 18px 15px;
    color: #ceafff;
    border: none;
    font-weight: 600;
    letter-spacing: 0.035em;
  }
  .styled-input__placeholder {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    z-index: -1;
    padding-left: 45px;
    color: white;
  }
  .styled-input__placeholder-text {
    -webkit-perspective: 500px;
    perspective: 500px;
    display: inline-block;
  }
  .styled-input__placeholder-text .letter {
    display: inline-block;
    vertical-align: middle;
    position: relative;
    -webkit-animation: letterAnimOut 0.25s ease forwards;
    animation: letterAnimOut 0.25s ease forwards;
    text-shadow: 0 0 5px;
  }
  .styled-input__placeholder-text .letter.active {
    -webkit-animation: letterAnimIn 0.25s ease forwards;
    animation: letterAnimIn 0.25s ease forwards;
  }
  .styled-input:hover {
    border-color: rgba(255, 255, 255, 0.4);
  }
  .styled-input.filled {
    border-color: rgba(255, 255, 255, 0.2);
  }
  .styled-input.filled .styled-input__circle:after {
    -webkit-transform: scale(37);
    transform: scale(37);
    opacity: 0;
  }
  @-webkit-keyframes letterAnimIn {
    0% {
      -webkit-transform: translate(0, 0);
      transform: translate(0, 0);
    }
    25% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      color: red;
    }
    45% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
      color: red;
    }
    55% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
    }
    56% {
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
      opacity: 0;
      color: #00ff6b;
    }
    76% {
      color: #00ff6b;
      opacity: 1;
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
    }
    100% {
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
      opacity: 1;
    }
  }
  @keyframes letterAnimIn {
    0% {
      -webkit-transform: translate(0, 0);
      transform: translate(0, 0);
    }
    25% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      color: red;
    }
    45% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
      color: red;
    }
    55% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
    }
    56% {
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
      opacity: 0;
      color: #00ff6b;
    }
    76% {
      color: #00ff6b;
      opacity: 1;
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
    }
    100% {
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
      opacity: 1;
    }
  }
  @-webkit-keyframes letterAnimOut {
    0% {
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
      opacity: 1;
    }
    25% {
      -webkit-transform: translate(-30px, -40px);
      transform: translate(-30px, -40px);
      opacity: 0;
    }
    45% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
    }
    55% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
      color: red;
    }
    56% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      color: red;
    }
    100% {
      -webkit-transform: translate(0, 0);
      transform: translate(0, 0);
    }
  }
  @keyframes letterAnimOut {
    0% {
      -webkit-transform: translate(-30px, -27px);
      transform: translate(-30px, -27px);
      opacity: 1;
    }
    25% {
      -webkit-transform: translate(-30px, -40px);
      transform: translate(-30px, -40px);
      opacity: 0;
    }
    45% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
    }
    55% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      opacity: 0;
      color: red;
    }
    56% {
      -webkit-transform: translate(0, 10px);
      transform: translate(0, 10px);
      color: red;
    }
    100% {
      -webkit-transform: translate(0, 0);
      transform: translate(0, 0);
    }
  }
  .spinner {
    position: relative;
    margin: auto;
    width: 50px;
    height: 50px;
    -webkit-transition: all 0.2s ease 0s;
    transition: all 0.2s ease 0s;
  }
  .spinner__circular {
    -webkit-animation: rotate 1.5s linear infinite;
    animation: rotate 1.5s linear infinite;
    -webkit-animation-play-state: paused;
    animation-play-state: paused;
    -webkit-transform-origin: center center;
    transform-origin: center center;
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    margin: auto;
  }
  .spinner__path {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
    -webkit-animation: dash 1.3s ease forwards 0.5s;
    animation: dash 1.3s ease forwards 0.5s;
    opacity: 0;
    stroke-linecap: round;
    stroke: #130826;
    -webkit-animation-play-state: running;
    animation-play-state: running;
  }
  @-webkit-keyframes dash {
    0% {
      stroke-dasharray: 1, 200;
      stroke-dashoffset: 0;
      opacity: 0;
    }
    50% {
      stroke-dasharray: 40, 200;
      opacity: 1;
    }
    100% {
      stroke-dasharray: 125, 200;
      opacity: 1;
    }
  }
  @keyframes dash {
    0% {
      stroke-dasharray: 1, 200;
      stroke-dashoffset: 0;
      opacity: 0;
    }
    50% {
      stroke-dasharray: 40, 200;
      opacity: 1;
    }
    100% {
      stroke-dasharray: 125, 200;
      opacity: 1;
    }
  }

</style>
