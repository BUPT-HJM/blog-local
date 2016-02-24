title: express+ajax小练习
date: 2016-02-23
categories:
  - 前端
  - nodejs
tags:
  - ajax
  - express
---

使用express-generator与jq的ajax实现的小例子，加深对ajax与表单提交的理解，适合初学者
<!--more-->

## **安装express**

```
npm install -g express-generator
```

## **新建工程**

```
express
npm install
npm start
//访问localhost：3000即可看到页面
```

## **修改引擎和视图**

在工程目录下的`app.js`修改为
```
//app.set('view engine', 'jade');
app.set('view engine', 'ejs');
```

>由于我个人比较习惯用ejs，ejs比较贴近html，html甚至改下扩展名就可以，jade比较简洁但是就跟html相差比较多了

修改工程目录中`route`目录下的`index.ejs`写我们的页面结构

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="./javascripts/jquery.min.js"></script>
</head>
<body>
    <div class="join-form">
                <form name="form" action="submit" method="get">
                    <div class="form">
                        <div class="form-item">
                            <input type="text" name="name" required="required" placeholder="姓名" autocomplete="off">
                        </div>
                        <div class="form-item">
                            <input type="text" name="institute" required="required" placeholder="学院" autocomplete="off">
                        </div>
                        <div class="form-item">
                            <input type="text" name="department" required="required" placeholder="意向部门" autocomplete="off">
                        </div>
                        <div class="form-item">
                            <input type="text" name="contact" required="required" placeholder="联系方式" autocomplete="off">
                        </div>
                        <textarea placeholder="请在此处介绍自己..." name="introduction">
                        </textarea>
                        <div class="button-panel">
                            <input type="submit" class="button" title="Sign In" value="报名">
                        </div>
                    </div>
                </form>
                <br>
                <button class="ajax">ajax</button>
                <div id="ajax"></div>
        </div>
</body>
<script src="./javascripts/ajax.js"></script>
</html>
```

>只是小练习不做css的修饰

**注意：**上面引入的js文件是放在public文件夹下的，但是引入并不需要`../public/XXX`

## **表单提交部分服务端编写**

在`index.js`加上

```
router.get("/submit",function(req,res){
     res.render('submit', {
        name: req.query.name,
        institute: req.query.institute,
        department: req.query.department,
        contact: req.query.contact,
        introduction: req.query.introduction
     });

});
```

当然渲染`submit`，我们也要在`views`下加上一个`submit.ejs`

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div>
        姓名：<%=name %><br>
        学院：<%=institute %><br>
        意向部门：<%=department %><br>
        联系方式：<%=contact %><br>
        介绍：<%=introduction %><br>
    </div>
</body>
</html>
```

## **ajax部分完成**

**jquery的ajax**
```
$('.ajax').on('click', function(event) {
    event.preventDefault();
    $.ajax({
        url: '/ajax',
        type: 'get',
        dataType: 'json',
        success:function(data){
            $('#ajax').html(data.tips);
        },
        error:function(data){
            alert('error');
        }
    });

});
```

**服务器端的响应**

在`index.ejs`添加
```
router.get('/ajax',function(req,res){
  var ajaxTest={
    tips:"ajax成功"
  };
  res.send(ajaxTest);
});
```

## **实现最终效果**

**未做任何操作**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/UC%E6%88%AA%E5%9B%BE20160224212035.png" alt="">
**点击ajax**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/UC%E6%88%AA%E5%9B%BE20160224212047.png" alt="">
**输入表单**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/UC%E6%88%AA%E5%9B%BE20160224212132.png" alt="">
**提交后的页面渲染**
<img src="http://7xp9v5.com1.z0.glb.clouddn.com/UC%E6%88%AA%E5%9B%BE20160224212200.png" alt="">

## **代码分享**

访问：https://github.com/BUPT-HJM/express-ajax

---

>可自由转载、引用，但需署名作者且注明文章出处。






