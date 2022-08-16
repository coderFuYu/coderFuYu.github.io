---
title: Vue使用阿里SDK上传文件至OSS
date: 2020-11-09 11:03:47
tags: [OSS,教程]
categories: 教程
keywords: OSS
top_img: https://oss.fycoder.top/Blog/OSSTopPic.jpeg
cover: https://oss.fycoder.top/Blog/OSSTopPic.jpeg
---

这个是一个Vue中用阿里云提供的SDK实现上传文件至OSS的demo，只有功能实现，没有美观度可言，希望给大家提供一点帮助
#  1.安装依赖
```bash
npm install ali-oss --save
```
#  2.代码
```html
<template>
  <div id="app">
    <input type="file" @change="getInfo($event)">
    <!--    文件路径显示-->
    <h2>path:{{ path }}</h2>
    <button @click="put">上传</button>
  </div>
</template>

<script>
const oss = require('ali-oss');
const client = oss({//设置OSS参数,输入自己的accessKeyId、accessKeySecret、bucket和region
  accessKeyId: 'your access key',
  accessKeySecret: 'your access secret',
  bucket: 'your bucket name',
  region: 'your region'
});

export default {
  name: 'App',
  data() {
    return {
      path: '',
      OSSPath: 'test/',//要上传至OSS的路径
      file: {}
    }
  },
  methods: {
    getInfo(e) {//选择文件
      this.path = e.target.value;
      this.file = e.target.files[0]
    },
    async put() {
      if (this.path !== '') {//判断文件非空
        let textArray=this.path.split('.')
        let extension = '.' + textArray[textArray.length-1]//获取扩展名
        try {
          let result = await client.put(this.OSSPath + Date.parse(new Date()) + extension, this.file);//此处文件命名使用的为时间戳防止冲突，可自行修改规则
          console.log(result);
          console.log(result.url);//完整的文件url
          alert('上传成功')
        } catch (e) {
          console.log(e);
          alert("上传失败")
        }
      }
      else{
        alert("请先选择要上传文件")
      }
    }
  }
}
</script>

<style>
</style>

```
