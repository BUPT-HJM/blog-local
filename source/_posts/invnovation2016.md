title: 大创项目提交与投票系统之前端小结
date: 2016-05-22
categories:
  - 前端
  - 项目小结
tags:
  - 前端
---

在4月份接手了这个项目，此次大创网站基于去年的网站基础改进，前端部分全面改版，功能方面增加项目分类、项目提交、在线抽奖等功能，虽然网站在上线过程中遇到了问题，不过也算最终圆满完成使命。写此文章来记录与大创技术组小伙伴一起奋斗的一个月~记录下项目遇到的坑~
<!--more-->

## **最终上线页面**
>不多说话，先放图
>前端部分采用bootstrap框架，完成对手机的适配，前端部分我负责项目分页展示页面，项目详细页面、项目提交页面

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E6%8D%95%E8%8E%B7.PNG" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/FireShot%20Capture%209%20-%20%E5%A4%A7%E5%88%9B%E5%B1%95%E6%8A%95%E7%A5%A8%E7%B3%BB%E7%BB%9F%20-%20http___10.3.240.199_projectSearch.action_Judge%3D0.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/FireShot%20Capture%2010%20-%20%E5%A4%A7%E5%88%9B%E6%8F%90%E4%BA%A4%E7%B3%BB%E7%BB%9F%20-%20http___10.3.240.199_login.jsp.png" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E6%89%8B%E6%9C%BA.PNG" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/2.PNG" alt="">
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/%E9%A1%B9%E7%9B%AE%E5%B1%95%E7%A4%BA%E9%A1%B5.PNG" alt="">

## **开发中问题以及解决问题**

### **如何完成图片上传，并且在其前端方面进行上传格式的限制**

由于面向的群体为大创参展项目的同学们，所以文件上传方面直接采用formdata进行上传，
formdata需要chrome7+，IE10+，Safari5+或其他浏览器高速模式才可以正常兼容，这个在页面上提示即可。注意得在form表单中加入`enctype="multipart/form-data"`，同时在`jquery ajax`提交中加入
```
  processData: false,  // 告诉jQuery不要去处理发送的数据
  contentType: false   // 告诉jQuery不要去设置Content-Type请求头
```
否则会造成提交失败。

然后对于格式判断，我直接采用正则表达式
```
var img = document.getElementById("submit-img").files[0];//获取图片信息
var img_type = img.type;
var img_test = /jpg|jpeg|png/g;
if(!img_test.test(img_type)) { //判断格式中是否带有jpg、jpeg、png字样
    //响应代码
}
```

### **在项目组上传不同尺寸图片，如何优雅地展示**

这需要在css方面稍微下点功夫，我对每个项目缩略图外做了高度固定栅格系统的小格子，用`width: 100%;`，`height`不加以设置，来保证图片正常的宽高比，用`overflow: hidden`来截取图片，在牺牲某些图片在缩略图下的效果，来保证整体的美观。同时用css3的`transition`结合`hover`，同时`position`的`relative`与`absolute`定位，来实现hover动态遮罩层，来提升用户PC端体验。移动端方面通过`.col-sm-* `类在移动端换得更好的显示。


### **项目分页的实现以及正则表达式的快速应用解决问题**
本来打算用bootstrap的分页插件解决，后来由于时间仓促，延续去年的方式，在jsp页面直接用`s:iterator`与`s:property`j解决了分页部分
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/FireShot%20Capture%209%20-%20%E5%A4%A7%E5%88%9B%E5%B1%95%E6%8A%95%E7%A5%A8%E7%B3%BB%E7%BB%9F%20-%20http___10.3.240.199_projectSearch.action_Judge%3D0.png" alt="">

>之前都觉得正则很少用，用在爬虫方面居多，在这次开发中，觉得正则表达式重要而且强大

由于后台数据库存取图片地址时存在问题，我想起前端方面可以迅速搞定问题，便直接在前端取出地址时正则表达式截取字符串，虽然简单，但是迅速解决了问题,后来发现移动端在图片src指定加载失败后，后再用js来替换src不能正常加载，便采取了先提取后端给出的地址，再存入src，解决了问题。

```
    <script>
    $(function(){             
              var src = $(".detail-img-src").text();
                var re = "C:\\vote\\InnovationVote\\";
                src = src.replace(re,"");
                $(".project-detail-img-box").find("img")[0].src = src;
                        
                        
                        
                var rr = /picture/;
              var src = $(".project-detail-img-box").find("img")[0].src;
              if(!rr.test(src))
                  $(".project-detail-img-box").find("img")[0].src = "/images/logo.png";
                }
    )
    </script>
```



### **web安全问题**
首先位于内网也算一个小安全吧，本来打算采用验证码，由于时间仓促，便取消了验证码，延续去年用刷卡取得有效的投票资格。但是由于小伙伴在对免卡登录的嘉宾号采用了错误的方式，没有采用随机数，导致在开展第二天出现脚本刷票情况（谁叫我们是在北邮hhh），最后当天晚上我们也紧急处理了情况，删除了刷票的异常数据，采用了新的嘉宾号码，最后解决了问题。觉得明年应该处理ip频繁请求问题，否则会导致网站异常问题。觉得可以`nginx`来处理频繁请求ip以及负载均衡问题。。

### **高并发或数据流问题**
由于这个项目基于去年的基础开发，忽略了这个方面的处理，由于平时对这方面经验也颇少，无法进行测试，导致网站在开展过程中出现错误情况，但是也以重启服务予以解决。（觉得主要还有这个鬼windows服务器的锅hhh）


>后端同学也异常辛苦，熬了好多个夜，也解决了很多棘手的问题，比如一些后台抽奖与投票逻辑等等，本文侧重个人前端方面，其他就不多说了

#### **总结**

在这个访问量达到10000+的网站的开发中，也是我第一次跟java后端进行交互，真的丰富了很多实际开发经验。
最后，感谢大创技术组的各个小伙伴，一起奋斗解决问题的感觉真是很好，最后也是圆满完成任务，hhh有种像士兵平时训练然后最后一起上战场然后最后完成胜利的感觉hhh，所以嘛，必须记录一下。
>恩恩，继续放图啊~(我还是那么凶，恩恩hh你说的没错)

<img src="http://7xp9v5.com1.z0.glb.clouddn.com/IMG_0897.JPG" alt="">




---

>可自由转载、引用，但需署名作者且注明文章出处。