<template>
  <div class="content_container">
    <vue-canvas-poster :widthPixels="1000" :painting="painting" @success="success" @fail="fail"></vue-canvas-poster>
    <img :src="posterImg">
  </div>
</template>

<script>
export default {
name: "Poster",
  data(){
    return{
      posterImg: '',//生成的海报
      painting: {
        width: '550px',
        height: '876px',
        background: 'https://ddcz.oss-cn-beijing.aliyuncs.com/images/file-1587004539522R5oa.png',
        views: [{
          type: 'image',
          url: 'http://thirdwx.qlogo.cn/mmopen/vi_32/Q0j4TwGTfTIxbEZu0fBQOYlTLTpqtoezkNiaCBQ4ZXZAKhvPiaFAdI91vBsUkpiaDH4mp4qGvcVo2EaxRrp5fMia8w/132',
          css: {
            top: '20px',
            left: '36px',
            borderRadius: '40px',
            width: '80px',
            height: '80px',
          },
        }, {
          type: 'text',
          text: '我的名字',
          css: {
            top: '48px',
            left: '136px',
            width: '360px',
            maxLines: 1,
            fontSize: '26px',
          },
        }, {
          type: 'qrcode',
          content: 'https://zy.jishiyoo.com/m/card/group?key=5PCxqmV7oF3D0wx3WnbnPLRlbunt7A',
          css: {
            bottom: '50px',
            right: '26px',
            color: '#000',
            background: '#fff',
            width: '130px',
            height: '130px',
            borderWidth: '10px',
            borderColor: '#fff'
          },
        },],
      },
    }
  },
  methods : {
    //成功生成海报
    success(src) {
      this.posterImg = src;
      console.log(src)
    },
    fail(err) {
      alert(err)
    },
  }
}
</script>

<style scoped>

</style>


<!--一个完整的这个没有问题，可以直接生成海报-->
















<!--<template>-->
<!--  <div>-->
<!--    <canvas id="myCanvas" width="360" height="570"></canvas>-->
<!--    <button type="button" @click="save">保存</button>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--export default {-->
<!--  mounted() {-->
<!--    var c = document.getElementById("myCanvas");-->
<!--    var ctx = c.getContext("2d");-->
<!--    var img1 = new Image();-->
<!--    img1.src = "https://picsum.photos/id/1016/800/450"; //背景图-->
<!--    img1.onload = function() {-->
<!--      ctx.drawImage(img1, 0, 0, 360, 570)-->
<!--      var img2 = new Image();-->
<!--      img2.src = "https://picsum.photos/id/1016/800/450"-->
<!--      img2.onload = function() {-->
<!--        ctx.drawImage(img2, 70, 70, 240, 320)-->
<!--        ctx.fillStyle = '#1296db';-->
<!--        ctx.textAlign = 'center';-->
<!--        ctx.font = 'normal 29px 楷体';-->
<!--        ctx.fillText('好友召集令', 190, 50);-->
<!--        ctx.fillStyle = '#000000';-->
<!--        ctx.font = 'normal 25px 宋体';-->
<!--        ctx.fillText('邀请码', 190, 130)-->
<!--        ctx.font = 'normal 15px 宋体';-->
<!--        ctx.fillText('长按也可保存分享哟~', 190, 410);-->
<!--        var base = "请求的base64资源"-->
<!--        var img3 = new Image();-->
<!--        img3.src = base;-->
<!--        img3.onload = function() {-->
<!--          ctx.drawImage(img3, 90, 180, 200, 200);-->
<!--        }-->
<!--      }-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    saveAsPNG(canvas) {-->
<!--      return canvas.toDataURL("image/png");-->
<!--    },-->
<!--    downLoad(url) {-->
<!--      var oA = document.createElement("a");-->
<!--      oA.download = ''; // 设置下载的文件名，默认是'下载'-->
<!--      oA.href = url;-->
<!--      document.body.appendChild(oA);-->
<!--      oA.click();-->
<!--      oA.remove(); // 下载之后把创建的元素删除-->
<!--    },-->
<!--    save() {-->
<!--      this.downLoad(this.saveAsPNG(myCanvas));-->
<!--    }-->
<!--  }-->
<!--}-->
<!--</script>-->

<!--<style>-->
<!--</style>-->



<!--一个完整的这个没有问题，可以直接生成海报-->














<!--<template>-->
<!--<div class="bg">-->
<!--  <img src="https://picsum.photos/id/1016/800/450" class="xk">-->
<!--  <br>-->
<!--  <br>-->
<!--  <br>-->
<!--  <div class="xk div">-->
<!--    <img src="https://picsum.photos/id/1016/800/450">-->
<!--    龙猫-->
<!--  </div>-->
<!--</div>-->
<!--</template>-->

<!--<style>-->
<!--.bg{-->
<!--  background-image: url("https://picsum.photos/id/1016/800/450");-->
<!--}-->
<!--img.xk{-->
<!--  position: absolute;-->
<!--  background-color: black;-->
<!--  width: 300px;-->
<!--  height: 200px;-->
<!--  border: 6px solid white;-->
<!--  box-shadow: 1px 1px 5px #333333;-->
<!--}-->
<!--.div{-->
<!--  position: absolute;-->
<!--  left: 50px;-->
<!--  top: 300px;-->
<!--  text-align: center;-->
<!--  font-size: 15px;-->
<!--  padding: 10px;-->
<!--  border: 1px solid #990;-->
<!--  background-color: white;-->
<!--  box-shadow: 1px 1px 5px #333333;-->
<!--  width: 200px;-->
<!--  transform: rotate(30deg);-->
<!--}-->
<!--img{-->
<!--  width: 100%;-->
<!--}-->
<!--</style>-->


<!--一个完整的有点瑕疵 是通过css控制的样式-->
