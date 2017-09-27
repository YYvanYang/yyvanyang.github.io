---
layout: post
title:  "移动端web开发Canvas画图的那些坑"
---

# 问题1：
当我们通过构造一个image对象的时候，并用服务器端返回的图片（*base64格式*）作为这个image对象的src的图象源时，image的onload事件在某些设备上会加载失败。

```js
let imgSourceData = api.getBase64() // 从服务器端返回的base64格式问图片
let img = new Image()
img.onload = function () {
    ...
}

img.src = imgSourceData
```

方案：
在页面上创建一个img标签

```html
<img id="imageSource" src="imgSourceData">
```

```js
let imgSourceData = api.getBase64() // 从服务器端返回的base64格式问图片
let img = new Image()
img.onload = function () {
    ...
}
let src = document.getElementById('imageSource').src
img.src = src
```

这样就可以正常调用img的onload事件。

总结：此问题主要由base64格式的图片源直接设置到image.src上而无法触发onload事件

# 问题2：
Canvas画图（多张图片合并）时，比例失调，总是无法准确计算图片比例的问题。

方案：
1，需要将各个图片按照设计稿的排版在页面上布局好。
2，通过getBoundingClientRect()方法获得元素的长和宽。
3，获得设备的设备像素比window.devicePixelRatio

```
将canvas的长宽 = 最终容器的长宽（由getBoundingClientRect()获得）* window.devicePixelRatio
```
此步主要是为了解决显示图片的清晰度问题。

然后通过getBoundingClientRect()获得各个子元素（图片）的长宽 * window.devicePixelRatio去调用canvas的drawImage方法。

这样合并出的图片跟设计上的才会吻合。

```js

var img2 = vm.$refs.refPhoto;
      
      img2.onload = function () {
        let devicePixelRatio = window.devicePixelRatio
        
        var expectWidth = vm.domFinalPhotoRect.width;
        var expectHeight = vm.domFinalPhotoRect.height;
      
        var photoBarRectWidth = vm.domPhotoBarRect.width;
        var photoBarRectHeight = vm.domPhotoBarRect.height;

        var savingTextIntoPhotoWidth = vm.domSavingTextIntoPhotoRect.width;
        var savingTextIntoPhotoHeight = vm.domSavingTextIntoPhotoRect.height;

        var qrcodeexpectWidth = vm.domQrPhotoRect.width;
        var qrcodeexpectHeight = vm.domQrPhotoRect.height;

        var canvas = document.createElement("canvas");
        var ctx = canvas.getContext("2d");
        canvas.width = expectWidth * devicePixelRatio;
        canvas.height = expectHeight * devicePixelRatio;
    
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        ctx.fillStyle = "#ffffff";
        ctx.fillRect(0, 0, canvas.width, canvas.height)

        ctx.drawImage(this, 0, 0, canvas.width, vm.domPhotoRect.height*devicePixelRatio);

        ctx.drawImage(vm.$refs.savingTextIntoPhoto, 
        30, 
        vm.domPhotoRect.height * devicePixelRatio, 
        savingTextIntoPhotoWidth * devicePixelRatio, 
        savingTextIntoPhotoHeight * devicePixelRatio);


        let boxHeight = vm.domPhotoBarRect.height * devicePixelRatio
        let qrHeight = qrcodeexpectHeight * devicePixelRatio

        let TopGap = 0;
        let rightGap = 10;

        if (boxHeight > qrHeight) {
          TopGap = (boxHeight - qrHeight) / 2
        }

        rightGap = Math.max(10, TopGap)

        ctx.drawImage(vm.$refs.qr, 
        canvas.width - qrcodeexpectWidth * devicePixelRatio - rightGap, 
        vm.domPhotoRect.height * devicePixelRatio  + TopGap, 
        qrcodeexpectWidth * devicePixelRatio, 
        qrcodeexpectHeight * devicePixelRatio);
        

        var base64 = canvas.toDataURL("image/jpeg", 1);
        vm.finalPhoto = base64
  
      }



```
