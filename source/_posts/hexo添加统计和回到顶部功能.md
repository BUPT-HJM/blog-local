title: 博客优化（添加统计和回到顶部功能）
date: 2016-01-09
categories:
  - 前端
  - hexo
tags:
  - hexo
---
## **添加统计**

>添加统计也可以通过百度统计来实现，本博客采用不蒜子统计，几行代码解决一切
<!--more-->
**打开`layout`文件夹下的_`partial文`件夹，修改`footer.ejs`，修改为**

```
<footer id="footer">
  <div class="outer">
    <div id="footer-info">
        <div class="footer-left">
            &copy; <%= date(new Date(), 'YYYY') %> <%= config.author || config.title %>
        </div>
              本站总访问量<span id="busuanzi_value_site_pv"></span>次
              本站访客数<span id="busuanzi_value_site_uv"></span>人次
              本文总阅读量<span id="busuanzi_value_page_pv"></span>次
        <div class="footer-right">
            <a href="http://hexo.io/" target="_blank">Hexo</a>  Theme <a href="https://github.com/litten/hexo-theme-yilia" target="_blank">Yilia</a> by Litten
        </div>
    </div>
  </div>
</footer>
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js">
</script>
```

>如果你想修改计数方式或者样式，可以详见
>链接:http://ibruce.info/2015/04/04/busuanzi/

## **添加回到顶部功能**

>参照http://www.yehbeats.com/2015/04/06/hexo-topmenu/

## **多说评论框优化**

**1.在多说后台管理系统中修改为盖楼（或平铺或嵌套）模式**

**2.在多说后台管理系统中自定义CSS框中加入自定义的代码**

```
#ds-thread #ds-reset ul.ds-comments-tabs li.ds-tab a.ds-current{border:0;text-shadow:none}
#ds-thread #ds-reset .ds-highlight{font-family:Microsoft YaHei,Arial,Helvetica,sans-serif;font-size:14px;font-weight:700}
#ds-thread #ds-reset ul.ds-comments-tabs li.ds-tab a.ds-current:hover{color:#FFF;background:#37b8eb}
#ds-thread #ds-reset a.ds-highlight:hover{color:#FFF!important}
#ds-thread{padding-left:5px}
#ds-thread #ds-reset li.ds-post,#ds-thread #ds-reset #ds-hot-posts{overflow:visible!important}
#ds-thread #ds-reset .ds-post-self{padding:10px 0 10px 20px}
#ds-thread #ds-reset li.ds-post,#ds-thread #ds-reset .ds-post-self{border:0!important}
#ds-reset .ds-avatar,#ds-thread #ds-reset ul.ds-children .ds-avatar{top:15px;left:-30px;width:55px!important;height:55px!important;box-shadow:-1px 0 1px rgba(0,0,0,.15) inset!important;border-radius:50%!important;padding:0}
#ds-thread .ds-avatar a{display:inline-block;width:45px;height:45px;border:1px solid #b9baa6;border-radius:50%!important;background-color:#fff!important;margin:4px;padding:0}
#ds-thread .ds-avatar a:hover{border-color:#de5a4e}
#ds-thread .ds-avatar>img{border:1px solid #b9baa6;margin:4px}
#ds-thread #ds-reset .ds-replybox{box-shadow:none}
#ds-thread #ds-reset ul.ds-children .ds-replybox.ds-inline-replybox a.ds-avatar,#ds-reset .ds-replybox.ds-inline-replybox a.ds-avatar{left:-15px!important;top:0!important;width:45px!important;height:45px!important;background:0;box-shadow:none;padding:0}
#ds-reset .ds-replybox.ds-inline-replybox a.ds-avatar img{width:45px!important;height:45px!important;border-radius:50%!important}
#ds-reset .ds-replybox a.ds-avatar,#ds-reset .ds-replybox .ds-avatar img{width:45px!important;height:45px!important;border-radius:5px;padding:0}
#ds-reset .ds-avatar img{width:45px!important;height:45px!important;border-radius:50%!important;box-shadow:0 1px 3px rgba(0,0,0,0.22);-webkit-backface-visibility:hidden;-webkit-transition:.4s all ease-in-out;-moz-transition:.4s all ease-in-out;-o-transition:.4s all ease-in-out;-ms-transition:.4s all ease-in-out;transition:.4s all ease-in-out}
.ds-post-self:hover .ds-avatar img{-webkit-transform:rotate(360deg);-moz-transform:rotate(360deg);-o-transform:rotate(360deg);-ms-transform:rotate(360deg);transform:rotate(360deg)}
#ds-thread #ds-reset .ds-comment-body{background:#f0f0e3;border-radius:5px;box-shadow:0 1px 2px rgba(0,0,0,.15),0 1px 0 rgba(255,255,255,.75) inset;padding:15px 15px 15px 55px!important}
#ds-thread #ds-reset .ds-comments a.ds-user-name{font-weight:700;color:#696a52!important}
#ds-thread #ds-reset .ds-comments a.ds-user-name:hover{color:#D32!important}
#ds-thread #ds-reset #ds-bubble{display:none!important}
#ds-thread #ds-reset #ds-hot-posts{border:0}
#ds-reset #ds-hot-posts .ds-gradient-bg{background:0}
#ds-reset #ds-ctx .ds-ctx-entry{border-bottom:1px solid #e6e6e6;margin:0;padding:17px 8px}
#ds-reset #ds-ctx .ds-ctx-entry .ds-avatar{height:55px;left:-36px;top:-9px;width:55px;margin:0}
#ds-thread #ds-reset .ds-comment-body a{color:#999}
#ds-reset .ds-comment-body #ds-ctx{background-color:rgba(0,0,0,0.03);border-left:0;border-radius:5px;box-shadow:0 1px 2px rgba(0,0,0,0.2),0 1px 0 rgba(255,255,255,0.5) inset}
#ds-reset #ds-ctx .ds-ctx-entry .ds-ctx-head a{color:#696a52;font-weight:700}
#ds-reset #ds-ctx .ds-ctx-entry .ds-ctx-head a:hover{color:#D32}
#ds-thread #ds-reset ul.ds-children .ds-avatar img{width:45px!important;height:45px!important;max-width:100%}
#ds-thread #ds-reset ul.ds-children .ds-comment-body,#ds-thread.ds-narrow #ds-reset .ds-replybox{padding-left:55px}
#ds-thread #ds-reset .ds-comment-body p,#ds-reset #ds-ctx .ds-ctx-entry .ds-ctx-content{color:#787968}
/*以上为把头像变圆及增加旋转效果*/
#ds-thread .ds-powered-by{display:none!important;}
/*去除多说底部版权特效*/
```







