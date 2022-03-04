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
          <img src="../../../../webphoto2/src/assets/tubiao/1tb.png">
        </div>
        <div class="tp">
          大图预览
        </div>
      </div>
      <div class="tplb">
        <div class="tp">
          <img src="../../../../webphoto2/src/assets/tubiao/2tb.png">
        </div>
        <div class="tp">
          原图保存
        </div>
      </div>
      <div class="tplb">
        <div class="tp">
          <img src="../../../../webphoto2/src/assets/tubiao/3tb.png">
        </div>
        <div class="tp">
          下载方便
        </div>
      </div>
    </div>
    <div class="login-box1">
      <el-form ref="form" :model="form" :rules="rules" label-width="80px;" class="login-box">
        <div class="login-title">个人资料</div>
<!--        头像-->
<!--        <img src="../../assets/class1.png" height="660" width="1100"/>-->
        <el-form-item label="" prop="avatar"><h2>头像</h2>
          <img
            style="width: 100px; height: 100px;border-radius: 50px"
            src="../../../../webphoto2/src/assets/class1.png"
             >
        </el-form-item>
        <!--      签名-->
        <el-form-item label=" " prop="sign"><h2>签名</h2>
          <el-input type="text" placeholder="天行健，君子以自强不息" v-model="form.sign"/>
        </el-form-item>
        <!--      昵称-->
        <el-form-item label=" " prop="nickname"><h2>昵称</h2>
          <el-input type="text" placeholder="零点" v-model="form.nickname"/>
        </el-form-item>
        <!--      生日-->
        <el-form-item label=" " prop="username"><h2> 生日</h2>
          <el-col :span="11">
            <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
          </el-col>
        </el-form-item>
        <!--      个人说明-->
        <el-form-item label=" " prop="information"><h2> 个人说明</h2>
          <el-input type="text" placeholder="天行健，君子以自强不息" v-model="form.information"/>
        </el-form-item>
        <!--      按钮-->
        <el-form-item class="login-button">
          <el-button type="primary" plain @click="submitForm('form')">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="all"></div>
  </div>
</template>

<script>
import * as axios from 'axios'

export default {
  name: 'PersonalInformation',
  inject:['reload'],
  data: function () {
    return {
      dialogImageUrl: '',
      dialogVisible: false,
      form: {
        sign: '',
        nickname: '',
        information:''
      },
      rules: {
        sign: [
          {required: true, message: ' ', trigger: 'blur'},
          {min: 1, max: 80, message: '长度在 1 到 20 个字符', trigger: 'blur'}
        ],
        nickname: [
          {required: true, message: ' ', trigger: 'blur'},
          {min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur'}
        ],
      }
    }
  },

  methods: {
    gotolast(){
      // 点击跳转至上次浏览页面
      this.$router.go(-1);
    },
    gotolink(){
      //指定跳转地址
      this.$router.replace('/login');
    },
    submitForm: function (formName) {
      this.param = new FormData();
      this.param.append('user', this.$store.getters.getUser.username);
      this.param.append('sign',this.form.sign);
      this.param.append('nickname',this.form.nickname);
      this.param.append('information',this.form.information);
      let config = {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      };

      const _this = this;
      this.$refs[formName].validate((valid) => {
        if (valid) {
          axios.post('http://localhost:8086/personal', this.param)
            .then(function (resp){
              console.log(resp);
            });
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
  /*position:fixed;*/
  min-width: 1000px;
  z-index:-10;
  zoom: 1;
  background-image: url("../../../../webphoto2/src/assets/beijing12.jpg");
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
  /*position: fixed;*/
}
.login-box{
  /*position: relative;*/
  width: 450px;
  margin:100px auto;
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
.all{
  clear: both;
  width: 100%;
}
</style>
