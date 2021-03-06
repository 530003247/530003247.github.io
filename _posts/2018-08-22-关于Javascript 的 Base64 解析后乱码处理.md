---
layout:     post
title:      关于JavaScript 的 Base64 中文解析
subtitle:   Base64 关于JavaScript 中文
date:       2018-08-22
author:     Kevin
header-img: https://upload-images.jianshu.io/upload_images/2320147-4fb9c8406ecf732d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/615
catalog: true
tags:
    - 关于JavaScript
    - Base64
    - crypto-js
    - UTF-16
---

# 关于JavaScript 的 Base64 中文解析 乱码


#### 处理方案 1

UTF8页面的编码在存储时实际上还是用的UTF-16！

解决方法：解码后把utf8还原为utf16即可。
附上网上找的utf8与utf16互相转换的实现：

##### utf-16转utf8

```
utf16to8 : function(str) {

var out, i, len, c;

out = "";

len = str.length;

for(i = 0; i < len; i++) {

c = str.charCodeAt(i);

if ((c >= 0x0001) && (c <= 0x007F)) {

out += str.charAt(i);

} else if (c > 0x07FF) {

out += String.fromCharCode(0xE0 | ((c >> 12) & 0x0F));

out += String.fromCharCode(0x80 | ((c >> 6) & 0x3F));

out += String.fromCharCode(0x80 | ((c >> 0) & 0x3F));

} else {

out += String.fromCharCode(0xC0 | ((c >> 6) & 0x1F));

out += String.fromCharCode(0x80 | ((c >> 0) & 0x3F));

}

}

return out;

},

//utf-8转utf-16

utf8to16 : function(str) {

var out, i, len, c;

var char2, char3;

out = "";

len = str.length;

i = 0;

while(i < len) {

c = str.charCodeAt(i++);

switch(c >> 4)

{

case 0: case 1: case 2: case 3: case 4: case 5: case 6: case 7:

// 0xxxxxxx

out += str.charAt(i-1);

break;

case 12: case 13:

// 110x xxxx 10xx xxxx

char2 = str.charCodeAt(i++);

out += String.fromCharCode(((c & 0x1F) << 6) | (char2 & 0x3F));

break;

case 14:

// 1110 xxxx 10xx xxxx 10xx xxxx

char2 = str.charCodeAt(i++);

char3 = str.charCodeAt(i++);

out += String.fromCharCode(((c & 0x0F) << 12) |

((char2 & 0x3F) << 6) |

((char3 & 0x3F) << 0));

break;

}

}
return out;
}
```

#### 解决方案 2 

引用谷歌的crypto-js库

```
var parsedWordArray = CryptoJS.enc.Base64.parse(userInfoStr);

var info = parsedWordArray.toString(CryptoJS.enc.Utf8);
```
