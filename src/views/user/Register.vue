<!--注册-->
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
        <div class="login-title">欢迎注册</div>
        <!--      用户名-->
        <el-form-item label=" " prop="username"><h2>Username</h2>
          <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
        </el-form-item>
        <!--      密码-->
        <div class="login">
          <el-form-item label=" " prop="password"><h2>Password</h2><img @click="trans()" :src="imgUrl" alt="">
            <el-input type="password" id="psw" placeholder="请输入密码" v-model="form.password"/>
          </el-form-item>
        </div>
        <div class="login">
          <el-form-item label=" " prop="password"><h2>Enter the password again</h2><img @click="trans2()" :src="imgUrl2" alt="">
            <el-input type="password" id="psw2" placeholder="请输入密码" v-model="form.pwdtwo"/>
          </el-form-item>
        </div>
        <el-form-item class="login-button">

          <!--<el-button type="primary" plain @click="submitForm('form')">REGISTER</el-button>-->
          <button @click="submitForm('form')" type="button" class="styled-button"> <span class="styled-button__real-text-holder"> <span
            class="styled-button__real-text">Register</span> <span class="styled-button__moving-block face"> <span
            class="styled-button__text-holder"> <span class="styled-button__text">Register</span> </span> </span><span
            class="styled-button__moving-block back"> <span class="styled-button__text-holder"> <span
            class="styled-button__text">REGISTER</span> </span> </span> </span> </button>
          <div class="zc">
            <div class="wzw">已拥有账号？点击登录-></div>
            <el-button type="info" plain @click="gotolink()">login</el-button>
          </div>
        </el-form-item>
      </el-form>
    </div>


  </div>
</template>

<script>
  import * as axios from "axios";
  export default {
    name: "Register",
    inject:['reload'],
    data: function () {
      return {
        picVisble:false,
        imgArr:['test_1.png','test.png'],
        imgArr2:['test_1.png','test.png'],
        imgIndex:0,
        imgIndex2:0,

        dialogImageUrl: '',
        dialogVisible: false,
        form: {
          username: '',
          password: '',
          pwdtwo:'',
          // rememberMe: false,
          // code: '',
          // uuid: '',
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
          pwdtwo: [
            {required: true, message: '请再次输入密码', trigger: 'blur'},
            {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur'}
          ],
        }
      }
    },
    computed:{
      imgUrl:function () {
        return require("../../assets/tubiao/"+this.imgArr[this.imgIndex]);
      },
      imgUrl2:function () {
        return require("../../assets/tubiao/"+this.imgArr2[this.imgIndex2]);
      }
    },
    methods: {
      trans(){
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
      trans2(){
        let psw2 = document.getElementById('psw2');
        if (psw2.type === 'password') {
          psw2.type = 'text';
        }else{
          psw2.type = 'password';
        }
        this.imgIndex2--;
        if(this.imgIndex2 < 0){
          this.imgIndex2 = this.imgArr2.length - 1;
        }
      },
      gotolast(){
        // 点击跳转至上次浏览页面
        this.$router.go(-1);
      },
      gotolink(){
        //指定跳转地址
        this.$router.replace('/login');
      },
      submitForm: function (formName) {
        const _this = this;
        this.$refs[formName].validate((valid) => {
          if (valid) {
            axios.post('http://localhost:8086/userRegister', this.form)
              .then(function (resp){
                console.log(resp);
                if(resp.data === 'success'){
                  _this.$alert('注册成功！','OK',{
                    confirmButtonText:'确定',
                    callback: action => {
                      _this.$router.push("/login");
                    }
                  });
                }
                else if(resp.data === 'userexist') {
                  _this.$message({
                    message: '用户名已注册!',
                    type: 'warning'
                  });
                } else if (resp.data === 'pwddifferent') {
                  _this.$message({
                    message: '两次密码填写不一致!',
                    type: 'warning'
                  });
                } else{
                  _this.$message({
                    message: '出错啦，请重新输入',
                    type: 'warning'
                  });
                  _this.reload()
                }
              });
            console.log(this.ruleForm);
          } else {
            return false;
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
    margin-top: 0;
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
  /*密码*/
  .login2 {
    position: relative;
  }
  .login2 img{
    position: absolute;
    top: 5px;
    right: 0px;
    width: 20px;
    height: 20px;
  }
  /*按钮*/
  .styled-button {
    -webkit-appearance: none;
    -webkit-user-select: none;
    cursor: pointer;
    font-size: 14px;
    width: 40%;
    padding: 12px;
    outline: none;
    background: none;
    position: relative;
    color: #a0a0a0;
    border-radius: 3px;
    margin-bottom: 25px;
    border: none;
    text-transform: uppercase;
    font-weight: 500;
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
    box-shadow: 0 8px 8px rgba(0, 0, 0, 0.8);
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
</style>
