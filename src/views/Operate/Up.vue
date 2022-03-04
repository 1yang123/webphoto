<!--shangchuan-->
<template>
    <div>
      <ul>
        <li>
          <div class="tx">
            <img src="../../assets/tubiao/shangchuan1.png" alt="">
          </div>
          <h3>普通上传</h3>
          <span>Normal upload</span>
          <div class="hr"></div>
          <p>使用此类上传可以单个或批量自行选择上传类别，还可以根据心情填写描述信息</p>
          <div class="hr_2"></div>
          <a href="/upload">Link</a>
        </li>
        <li>
          <div class="tx">
            <img src="../../assets/tubiao/shangchuan11.png" alt="">
          </div>
          <h3>智能识别上传</h3>
          <span>Intelligent identification upload</span>
          <div class="hr"></div>
          <p>使用此类上传可以单张或批量上传图片，自动识别图片内容，可以展示识别结果并将图片存入相应类别相册，方便省心。</p>
          <div class="hr_2"></div>
          <a href="/loads">Link</a>
        </li>

      </ul>
    </div>
</template>

<script>
    export default {
        name: "Up"
    }
</script>

<style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;

  }
  body {
    height: 100vh;
    background-image: linear-gradient(120deg, #a1c4fd 0%, #c2e9fb 100%);


  }
  li {
    list-style: none;
  }
  a {
    text-decoration: none;
  }
  ul {
    width: 1040px;
    height: 500px;
    margin: 100px auto;

  }
  ul li {
    margin-top: 50px;
    position: relative;
    float: left;
    margin-left: 130px;
    width: 280px;
    height: 400px;
    background-color: #fff;
    border-radius: 15px;
    box-shadow: 4px 2px 3px 1px  rgba(0,0,0,.1);
    transition: all .5s;
  }
  ul li h3 {
    margin-top: 100px;
    text-align: center;
  }
  ul li span {
    margin-top: 10px;
    display: block;
    text-align: center;
    font-size: 12px;
  }
  ul li .hr {
    margin-top: 25px;
    width: 100%;
    height: 2px;
    background-image: linear-gradient(120deg, #69cefc 0%, #000000 100%);
  }
  ul li p {
    margin-top: 45px;
    margin-left: 15px;
    font-size: 14px;
  }
  ul li .hr_2 {
    margin-top: 65px;
    width: 100%;
    height: 2px;
    background: rgba(0,0,0,.1);
  }
  ul li  a{
    margin: 0;
    padding: 0;
    text-align: center;
    line-height: 50px;
    display: inline-block;
    color: #000;
    font-size: 14px;
    width: 137px;
    height: 50px;
    transition: all .3s;
    border-radius: 20px;

  }
  ul li a:hover{
    background-image: linear-gradient(135deg, #667eea 0%, #c3c3c3 100%);
    transform: scale(1.2);
  }
  ul li:hover {
    transform: scale(1.1);
  }
  ul li .tx{
    box-shadow: 1px 1px 5px 1px  rgba(0,0,0,.1);
    position: absolute;
    top: -80px;
    left: 65px;
    width: 150px;
    height: 150px;
    border-radius: 50%;
    background-color: #fff;
    transition: all .3s;
  }
  ul li .tx img{
    width: 90%;
    height: 90%;
    border-radius: 50%;
    display: block;
    margin-left: 8px;
    margin-top: 7px;
  }
  ul li .tx:hover {
    transform: scale(1.1) rotateZ(360deg);
  }
</style>
