<!--智能识别上传-->
<template>
  <div class="body">

    <!--//图片上传upload-->
      <div class="center">
        <div class="bd">

          <div class="anniu">
            <button @click="goto()" type="button" class="styled-button"> <span class="styled-button__real-text-holder"> <span
              class="styled-button__real-text"><-普通上传</span> <span class="styled-button__moving-block face"> <span
              class="styled-button__text-holder"> <span class="styled-button__text"><-普通上传</span> </span> </span><span
              class="styled-button__moving-block back"> <span class="styled-button__text-holder"> <span
              class="styled-button__text"><-普通上传</span> </span> </span> </span> </button>
          </div>
          <el-row>
            <el-col :span="20" :offset="3">
              <div id="upload">

                <div class="upload-image">
                  <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
                    <!--<el-form-item label="图片描述" prop="details">-->
                    <!--<el-input v-model="ruleForm.details" style="width: 80%"></el-input>-->
                    <!--</el-form-item>-->
                    <el-form-item label="上传图片" ref="uploadElement" prop="imageUrl">
                      <el-input v-model="ruleForm.imageUrl" v-if="false"></el-input>
                      <el-upload
                        action="https://jsonplaceholder.typicode.com/posts/"
                        list-type="picture-card"
                        :on-preview="handlePictureCardPreview"
                        :on-remove="handleRemove"
                        :multiple="true"
                        class="avatar-uploader"
                        ref="upload"
                        :before-upload="beforeUpload"
                        :on-change="handleChange"
                        :auto-upload="false"
                        :data="ruleForm">
                        <i class="el-icon-plus"></i>
                      </el-upload>
                      <el-dialog :visible.sync="dialogVisible">
                        <img width="100%" :src="dialogImageUrl" alt="">
                      </el-dialog>
                    </el-form-item>

                    <el-form-item>
                      <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                      <el-button @click="resetForm('ruleForm')">重置</el-button>
                      <el-button @click="downloadexcel('ruleForm')">下载excel表</el-button>
                    </el-form-item>
                  </el-form>
                </div>
                <div>

                </div>

              </div>
            </el-col>

          </el-row>
        </div>
      </div>

    <div class="zhanshi">
      <el-table
        highlight-current-row
        :data="tableData"
        tooltip-effect="dark"
        height="300"
        style="width: 300px;margin-left: 200px;float: left;display: inline-block"
        @cell-click="showtable">

        <el-table-column
          prop="name"
          label="图片名称"
          width="300">
          <template slot-scope="scope">
            {{scope.row.originname}}
          </template>
        </el-table-column>
      </el-table>

      <div class="tpzs">
        <!--<img :src="'../../../static/smart_photos/allimgs/'+item.nowname ? '../../../static/smart_photos/allimgs/'+item.nowname : squareUrl" style="width: 100%">-->
        <img :src="'../../../static/smart_photos/allimgs/'+item.nowname" style="width: 100%">
      </div>

      <div class="typeouter">
        <div class="type">图片类别</div>
        <div class="type1">{{item.type}}</div>
      </div>
      <!--<div class="typeouter">-->
      <!--<div class="type1">-->
      <!--<el-button @click="downloadexcel()">下载excel表</el-button>-->
      <!--</div>-->
      <!--</div>-->

      <div class="typeouter2">
        <div class="type">识别结果</div>
        <div class="type1">{{item.distingush}}</div>
      </div>
    </div>

    <div class="all"></div>
  </div>
</template>


<script>
  import * as axios from "axios";
  import { Loading } from 'element-ui';

  export default {
    name: "Page1_2",
    inject: ['reload'],
    data() {
      return {
        item:'',
        items:'',
        squareUrl:'',
        imgshow:'',
        tableData: [
          // {
          //   nowname:'bz1.jpg',type:'风景',originname:'bz1.jpg',
          // },
          // {nowname:'002.jpg',type:'动物'},{nowname:'003.jpg',type:'建筑'},{nowname:'004.jpg',type:'风景'},{nowname:'005.jpg',type:'风景'},
          // {
          //   nowname:'bz2.jpg',type:'风景'
          // },
          // {nowname:'bz3.jpg',type:'建筑'},{nowname:'bz4.jpg',type:'建筑'},{nowname:'bz5.jpg',type:'动物'},{nowname:'bz6.jpg',type:'动物'},
          // {
          //   name:'edcde',type:'风景'
          // },
          // {name:'2.jpg',type:'风景'},{name:'56789.png',type:'风景'},{name:'345678987ytdxdfghjkuytr4esxcvhuytfcvb',type:'风景'},{name:'r56tgjki8ikmn',type:'风景'},
        ],
        search: '',
        arr: [],
        imgPath: [],



        fileData: '',
        fileList: [],
        dialogImageUrl: '',
        dialogVisible: false,
        ruleForm: {
          details: '',
          imageUrl: '',
        },
        rules: {
          imageUrl: [
            {required: true, message: '请上传图片', trigger: 'blur'},
          ],
        },
        coomonlist: {
          filename:'',
          type:'',
          fileSize:'',
          master:'',
          date:'',
          faceSetId:'',
          faceId:'',
        },
        face: {
          filename:'',
          type:'',
          fileSize:'',
          master:'',
          date:'',
          faceSetId:'',
          faceId:'',
        },
        newface:{
          filename:'',
          type:'',
          fileSize:'',
          master:'',
          date:'',
          faceSetId:'',
          faceId:'',
        },

      }
    },
    created() {
      // 获取默认显示的图片
      this.squareUrl = require('../../assets/tubiao/mrtouxiang.png')
    },
    methods: {
      goto(){
        this.$router.push({ path:'/upload'
        })
      },
      showtable(row){
        console.log(row.originname);
        console.log(row.nowname);
        console.log(row.type);
        this.item = row;
        this.items = row.type;
        // showtype();
      },
      showtype(row){
        console.log(row.name);
        console.log(row.type);
        console.log(row.name);
        this.item = row;
        // showtype(row);
      },
      handleRemove(file, fileList) {
        // return this.$confirm("确定移除" +file.name+"？");
        console.log(file, fileList);
        this.reload()
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      },
      downloadexcel(formName) {
        this.param = new FormData();
        this.param.append('user', this.$store.getters.getUser.username);
        const _this = this;
        _this.$refs[formName].validate((valid) => {
          if (valid) {
            axios.post('http://localhost:8086/getExcelDownload', _this.param,{responseType:'blob'})
              .then(resp=> {
                console.log(resp);
                let data = resp.data;
                let url = window.URL.createObjectURL(new Blob([data]));
                let link = document.createElement('a');
                link.style.display = 'none';
                link.href = url;
                link.setAttribute('download', 'outcome.xls');
                document.body.appendChild(link);
                link.click()
              });
          }
        })
      },
      submitForm(formName) {
        const loading = this.$loading({
          lock: true,
          text: 'Loading',
          spinner: 'el-icon-loading',
          background: 'rgba(0, 0, 0, 0.7)'
        });
        this.param = new FormData();
        this.param.append('user', this.$store.getters.getUser.username);
        for (let item of document.querySelector('input[type=file]').files) {
          this.param.append('files', item);
        }// 'files' 这个名字要和后台获取文件的名字一样;
        // this.param.append('details', this.ruleForm.details);
        let config = {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        };
        console.log(this.param);
        const _this = this;
        this.$refs[formName].validate((valid) => {
          if (valid) {
            axios.post('http://localhost:8086/facedetect', _this.param,{
              headers:{
                authorization:localStorage.getItem('token')
              }
            })
              .then(function (resp) {
                console.log(resp);
                loading.close(); // 关闭加载
                _this.tableData = resp.data;
                // _this.coomonlist = resp.data.common;
                // _this.face = resp.data.face;
                // _this.newface = resp.data.newface;
                console.log(resp);
                // if (resp.status === 200) {
                //   try {
                //     console.log("../" + resp.data);
                //     if (typeof this.loadImage !== "undefined") {
                //       this.loadImage =
                //         "http:.../getImage/" + resp.data;
                //     }
                //   } catch (e) {
                //     console.log("捕获到异常：", e);
                //   }
                // } else {
                //   console.log("失败" + 400);
                // }

                // });
                // _this.$refs['ruleForm'].resetFields();
                // } else {
                //   return false;
                // }
              });
          }
        });
      },

      resetForm(formName) {
        this.$refs[formName].resetFields();
        this.ruleForm.imageUrl = '';
        this.reload()
      },
      handleChange(file, fileList) {
        this.ruleForm.imageUrl = URL.createObjectURL(file.raw);
      },

      beforeUpload(file) {
        return true;
      },
    }
  }

</script>

<style scoped>
  .all{
    clear: both;
    width: 100%;
  }
  .body{
    /*background-color: rgb(235, 235, 235);*/
    padding-top: 60px;
    padding-bottom: 60px;
    /*background-image: url('../../assets/001.jpg');*/
    background-image: url(../../assets/beijing1.jpg);
  }
  .center {
    text-align: center;
  }

  .bd {
    position:relative;
    padding-top: 60px;
    padding-bottom: 30px;
    background-color: #fff;
    margin-left: 24%;
    text-align: left;
    width: 900px;
  }

  input[type="file"] {
    display: none;
  }

  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }

  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }

  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }

  .avatar {
    width: 178px;
    height: 178px;
    display: none;
  }

  .tpzs{
    border-radius: 10px;
    width: 400px;
    height: 300px;
    float: left;
    display: inline-block;
    /*background-color: #9bb7ff;*/
    background-image: url('../../assets/005.jpg');
    margin-left: 20px;
    overflow: hidden;
  }
  .tpzs img{
    height: 100%;
    width: 100%;
    object-fit:cover;
    border-radius: 10px;
  }
  .typeouter{
    margin-top: 30px;
    width: 500px;
    float: left;
    margin-left: 20px;
    border: 1px solid black;
    border-radius: 6px;
    background-color: #fff;
  }
  .typeouter2{
    margin-top: 30px;
    width: 500px;
    float: left;
    margin-left: 20px;
    border: 1px solid black;
    border-radius: 6px;
    background-color: #fff;
  }
  .type{
    padding-left: 20px;
    width: 400px;
    line-height: 40px;
    color: #666666;
    font-size: 16px;
    border-bottom: 1px solid gainsboro;
  }
  .type1{

    padding-left: 20px;
    width: 400px;
    line-height: 40px;
    color: #000000;
    font-size: 20px;
  }
.zhanshi{
  width: 100%;
  margin-top: 100px;
}
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
    border: 1px solid #DDD;
    text-transform: uppercase;
    font-weight: 700;
    letter-spacing: 0.1em;
    background: #ffffff;
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
    box-shadow: 0 8px 20px rgba(132, 132, 132, 0.6);
    background: #9c9c9c;
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
  .anniu{
    position: absolute;
    top: 40px;
    left: -60px;
    width: 160px;
    height: 200px;
  }
</style>

