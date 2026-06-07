---
title: PDF处理技巧
categories: Other
abbrlink: PDF-Processing-Tips
date: 2021-10-11 15:03:57
tags:
---

![](topic.jpg)

PDF处理技巧。

<!-- more -->

# PDF去水印

## 边缘水印

位于页眉、页脚或其它边缘位置的水印，可以通过Adobe Acrobat DC编辑，通过裁切页面的方式去除。也可以用PDF Eraser，通过Delete Area功能选中每个页面需要删除的部分即可。

## 底部水印

对于添加在文字版的PDF文档上的水印或背景，若与正文颜色差别明显，则可有效去除。

如果页数不多，可以通过Adobe Acrobat DC编辑，按住Shift键多选要删除的水印，按Delete键将其去删除即可。

如果页数较多，可以利用Adobe Acrobat DC的分色打印功能批量删除。点击打印，打印机选择`Adobe PDF`，高级设置为`Adobe默认值`，颜色改为`分色`，叉掉`黑色`，右边下拉选择`确定`，然后点击`打印`。

用Adobe Acrobat DC打开打印好的PDF，点击视图-工具-组织页面，
选择组织内容，调整每一排显示三个页面，然后删除多余的两排页面即可。

也可使用Foxit PDF Editor。根据水印的类型，在选择类型里面选择水印对应的类型。以路径型水印为例，选择对象类型选择为路径，按Ctrl+A选中页面中所有相同类型的部分，按Delete键删除即可。

## 可检测水印

某些水印能被PDF Watermark Remover或万彩办公大师直接检测到，然后即可一键批量删除。万彩办公大师官网如下。

```
http://www.wofficebox.com/
```

也可先使用Adobe Acrobat DC打开PDF后，点击视图—工具—组织页面，把PDF拆分成每三页一个。然后打开PDF水印清理专家试用版，勾选移除图片上浅灰色背景水印，点击批量处理有同样水印的其他文件，选中拆分开来的PDF。去除完成后打开Adobe Acrobat DC，点击工具—合并文件，选中去除完水印的PDF文件，点击右上角合并即可。

PDF水印清理专家官网如下。

```
http://www.liyusoft.com/pdfwr
```

若编辑PDF时提示为加密文档，可尝试使用PDF Password Remover解密。

# 参考教程

## 你绝没看过如此详细的PDF去水印教程

```
https://mp.weixin.qq.com/s/uDGVK439x57UeUgmGRBSOQ
```

## 10款免费PDF利器：无水印、离线使用、全能转换、极致压缩…

```
https://mp.weixin.qq.com/s/nGPW935VcfhhKO7ODQh7TA
```

## 别再浪费钱了，福昕、微软、Adobe完全免费的PDF服务求你用一下！

```
https://mp.weixin.qq.com/s/oHLq0lnjKLUCZTJGC7FiNw
```
