<template>
  <div style="background-color: black">
    <div class="container">
      <div id="tui-image-editor"></div>
      <div style="color: black">
        <button @click="uploadImg" class="btt">完成并上传</button>
      </div>
    </div>
  </div>
</template>
<script>
import "tui-image-editor/dist/tui-image-editor.css";
import "tui-color-picker/dist/tui-color-picker.css";
const ImageEditor = require("tui-image-editor");
const locale_zh = {
  // override default English locale to your custom
  Crop: "裁剪",
  DeleteAll: "全部删除",
  Delete: "删除",
  Undo: "撤销",
  Redo: "反撤销",
  Reset: "重置",
  Flip: "镜像",
  Rotate: "旋转",
  Draw: "画",
  Shape: "形状标注",
  Icon: "图标标注",
  Text: "文字标注",
  Mask: "遮罩",
  Filter: "滤镜",
  Bold: "加粗",
  Italic: "斜体",
  Underline: "下划线",
  Left: "左对齐",
  Center: "居中",
  Right: "右对齐",
  Color: "颜色",
  "Text size": "字体大小",
  Custom: "自定义",
  Square: "正方形",
  Apply: "应用",
  Cancel: "取消",
  "Flip X": "X 轴",
  "Flip Y": "Y 轴",
  Range: "区间",
  Stroke: "描边",
  Fill: "填充",
  Circle: "圆",
  Triangle: "三角",
  Rectangle: "矩形",
  Free: "曲线",
  Straight: "直线",
  Arrow: "箭头",
  "Arrow-2": "箭头2",
  "Arrow-3": "箭头3",
  "Star-1": "星星1",
  "Star-2": "星星2",
  Polygon: "多边形",
  Location: "定位",
  Heart: "心形",
  Bubble: "气泡",
  "Custom icon": "自定义图标",
  "Load Mask Image": "加载蒙层图片",
  Grayscale: "灰度",
  Blur: "模糊",
  Sharpen: "锐化",
  Emboss: "浮雕",
  "Remove White": "除去白色",
  Distance: "距离",
  Brightness: "亮度",
  Noise: "噪音",
  "Color Filter": "彩色滤镜",
  Sepia: "棕色",
  Sepia2: "棕色2",
  Invert: "负片",
  Pixelate: "像素化",
  Threshold: "阈值",
  Tint: "色调",
  Multiply: "正片叠底",
  Blend: "混合色"
  // etc...
};
const customTheme = {
  // image 左上角度图片
  "common.bi.image": " ", // 在这里换上你喜欢的logo图片
  "common.bisize.width": "0px",
  "common.bisize.height": "0px",
  "common.backgroundImage": "none",
  "common.backgroundColor": "#141515",
  "common.border": "1px solid #444",

  // download button
  "downloadButton.backgroundColor": "#fdba3b",
  "downloadButton.border": "1px solid #fdba3b",
  "downloadButton.color": "#fff",
  "downloadButton.fontFamily": "NotoSans, sans-serif",
  "downloadButton.fontSize": "12px",
  "downloadButton.display": "none", // 可以直接隐藏掉
 // icons default
  "menu.normalIcon.color": "#8a8a8a",
  "menu.activeIcon.color": "#555555",
  "menu.disabledIcon.color": "#434343",
  "menu.hoverIcon.color": "#e9e9e9",
  "submenu.normalIcon.color": "#8a8a8a",
  "submenu.activeIcon.color": "#e9e9e9",

  // submenu primary color
  "submenu.backgroundColor": "#1e1e1e",
  "submenu.partition.color": "#858585",

  // submenu labels
  "submenu.normalLabel.color": "#858585",
  "submenu.normalLabel.fontWeight": "lighter",
  "submenu.activeLabel.color": "#fff",
  "submenu.activeLabel.fontWeight": "lighter",

  // checkbox style
  "checkbox.border": "1px solid #ccc",
  "checkbox.backgroundColor": "#fff",

  // rango style
  "range.pointer.color": "#fff",
  "range.bar.color": "#666",
  "range.subbar.color": "#d1d1d1",

  "range.disabledPointer.color": "#414141",
  "range.disabledBar.color": "#282828",
  "range.disabledSubbar.color": "#414141",

  "range.value.color": "#fff",
  "range.value.fontWeight": "lighter",
  "range.value.fontSize": "11px",
  "range.value.border": "1px solid #353535",
  "range.value.backgroundColor": "#151515",
  "range.title.color": "#fff",
  "range.title.fontWeight": "lighter",

  // colorpicker style
  "colorpicker.button.border": "1px solid #1e1e1e",
  "colorpicker.title.color": "#fff",

};
export default {
  data() {
    return {
      instance: null,
    }
  },
  mounted() {
    this.instance = new ImageEditor(
      document.querySelector("#tui-image-editor"),
      {
        includeUI: {
          loadImage: {
            path: "https://picsum.photos/id/1016/800/450",//图片地址，必须连接网络
            name: "image"
          },
          initMenu: "draw",
          menuBarPosition: "bottom",
          locale: locale_zh, // 本地化语言为中文
          theme: customTheme,// 自定义主题
        },
      }
    );
  },
methods: {
  uploadImg() {
    const base64String = this.instance.toDataURL();
    const data = window.atob(base64String.split(",")[1]);
    const ia = new Uint8Array(data.length);
    for (let i = 0; i < data.length; i++) {
      ia[i] = data.charCodeAt(i);                            // 这个插件提供了一个toDataURL的方法,
      // 可以将我们编辑后的图片转为base64,
      // 然后我们可以通过转为form表单上传到我们的服务器，
      //或者转为图片，这里提供一份上传到后台的逻辑：
    }
    const blob = new Blob([ia], { type: "image/png" });
    const fd = new FormData();
    fd.append("image", blob);
    // upload fd......这个是提示不用恢复，无用的
    // this.param = fd;
    // const _this = this;
    // axios.post('http://localhost:8086/...',_this.param).then(function (resp){
    //   console.log(resp)
    //   _this.reload()
    // });

  },
},
}
</script>
<style  scoped>
.container {
  /*height: 95vh;*/
  height: 700px;
  text-align: center;
  color: black;
}
.btt{
  margin-left: 680px;
  width: 118px;
  height: 50px;
  border-radius: 30px;
  position: fixed;
}
</style>
