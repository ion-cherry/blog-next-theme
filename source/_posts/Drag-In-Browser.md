---
title: Drag In Browser
date: 2020-07-29 18:10:51
tags:
    -
    -
categories:
    - web前端
---

# <center>原生JavaScript拖拽，移动端也可用</center>
* 我究竟忘了多少东西<!-- more -->
1. >一个html文件直接运行😁 
<details>
    <summary>原生drag&&touch</summary>
    ```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>touch_drag</title>
    <style>
        html {
            font-size: calc(100vw / 7.5);
            /* font-size: 50px; 
			 设计稿的px/100 */
            height: 100%;
            min-width: 320px;
            overflow-x: hidden;
			
        }
        body {
            font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
            font-size: .28em;
            line-height: 1;
            margin: 0
        }
        .contaienr {
            width:100%;
            height: 100%;
        }
        .box {
            width: 1rem;
            height: 1rem;
            background: purple;
            border-radius: 50%;
            position: absolute;
            top: 0;
            left: 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="box">

        </div>
    </div>

    <script>
        window.onload = function() {
                
            
            const box = document.querySelector('.box')
            
            function dragEle(ele) {
                ele.onmousedown = function(evt) {
                    let e = evt || window.event
                    let disX = e.clientX - ele.offsetLeft
                    let disY = e.clientY - ele.offsetTop
                    
                    document.onmousemove = function(evt) {
                        let e = evt || window.event
                        let left = e.clientX - disX
                        let top = e.clientY - disY
                        
                        let leftMax = window.innerWidth - ele.offsetWidth
                        let topMax = window.innerHeight - ele.offsetHeight

                        if(left <= 0) {
                            left = 0
                        }else if(left >= leftMax) {
                            left = leftMax
                        }

                        if(top <= 0) {
                            top = 0
                        }else if(top >= topMax) {
                            top = topMax
                        }

                        ele.style.left = left + 'px'
                        ele.style.top = top + 'px'

                    }

                    document.onmouseup = function(e) {
                        this.onmousemove = null
                        this.onmouseup = null
                    }
                }
            }
            
            function touchEle(ele) {

                ele.ontouchstart = function(evt) {
                    let e = evt || window.event
                    let touchX = e.changedTouches[0].clientX - ele.offsetLeft
                    let touchY = e.changedTouches[0].clientY - ele.offsetTop

                    document.ontouchmove = function(evt) {
                        let e = evt || window.event
                        let left = e.changedTouches[0].clientX - touchX
                        let top = e.changedTouches[0].clientY - touchY

                        let leftMax = window.innerWidth - ele.offsetWidth
                        let topMax = window.innerHeight - ele.offsetHeight

                        if(left <= 0) {
                            left = 0
                        }else if(left >= leftMax) {
                            left = leftMax
                        }

                        if(top <= 0) {
                            top = 0
                        }else if(top >= topMax) {
                            top = topMax
                        }

                        ele.style.left = left + 'px'
                        ele.style.top = top + 'px'
                    }

                    document.ontouchend = function(evt) {
                    this.ontouchmove = null
                        this.ontouchend = null
                    }
                }

            }
        
            const a = navigator.userAgent
            console.log(a, navigator)
            if(a.indexOf("Android") != -1 || a.indexOf("iPhone") != -1 || a.indexOf("iPad") != -1) {
                touchEle(box)
                
            }else {
                dragEle(box)
                
            }
        }
		
    </script>
</body>
</html>

```
</details>

2. > 要怎么封装呢，还在考虑中🤔

3. 代码解析
    > Css布局参考这里  [rem布局](https://www.cnblogs.com/olive27/p/6593126.html)

        html{
        　　font-size:calc(100vw/7.5);
        }

        这是按照750的设计稿（也就是iphone6的设计稿）。

        100vw是设备的宽度，除以7.5可以让1rem的大小在iPhone6下等于50px。

        替换页面中的单位，把所有的px单位替换成rem，除以100，比如某字体大小在设计稿上是36px，就是0.36rem。

        在iPhone6下，所有元素的尺寸还是和视觉稿的尺寸一样，而iphone5/iphone6plus中，因为设备的宽度变小/变大了，100vw/7.5得到的值，会相应的变小、变大，即rem的单位值会变，页面中所有的尺寸会等比例缩放。

        so,这样就做到l了针对不同分辨率的设备保持视觉一致了。

