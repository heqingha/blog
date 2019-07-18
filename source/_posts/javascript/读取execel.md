---
title: 行为型设计模式
categories:
  - web skill
tags:
  - js
date: 2017-12-05 10:41:20
---

```js
// 导入excel文件
importfxx(obj) {
let _this = this;
let inputDOM = this.$refs.inputer;
// 通过DOM取文件数据
this.file = document.getElementById('uploadExcel').files[0]; //event.currentTarget.files[0];
var rABS = false; //是否将文件读取为二进制字符串
var f = this.file;
var reader = new FileReader();
FileReader.prototype.readAsBinaryString = function(f) {
var binary = '';
var rABS = false; //是否将文件读取为二进制字符串
var pt = this;
var wb; //读取完成的数据
var reader = new FileReader();
reader.onload = function(e) {
var bytes = new Uint8Array(reader.result);
var length = bytes.byteLength;
for (var i = 0; i < length; i++) {
binary += String.fromCharCode(bytes[i]);
}
var XLSX = require('xlsx');
if (rABS) {
wb = XLSX.read(btoa(fixdata(binary)), {
//手动转化
type: 'base64'
});
} else {
wb = XLSX.read(binary, {
type: 'binary'
});
}
_this.outdata = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]); //outdata就是你想要的东西
};
reader.readAsArrayBuffer(f);
};
if (rABS) {
reader.readAsArrayBuffer(f);
} else {
reader.readAsBinaryString(f);
}
},
```
