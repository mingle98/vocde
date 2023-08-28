


**前端必备工具推荐网站(免费图床、API和ChatAI等实用工具):**
[http://luckycola.com.cn/](http://luckycola.com.cn/)


# 前言
一款免费的前端字符验证码API,该api是直接可用的,可自定义配置的.

验证码（CAPTCHA）是“Completely Automated Public Turing test to tell Computers and Humans Apart”（全自动区分计算机和人类的图灵测试）的缩写，
是一种区分用户是计算机还是人的公共全自动程序。可以防止：恶意破解密码、刷票、论坛灌水，有效防止某个黑客对某一个特定注册用户用特定程序暴力破解方式进行不断的登陆尝试。
这个问题可以由计算机生成并评判，但是必须只有人类才能解答。由于计算机无法解答CAPTCHA的问题，所以回答出问题的用户就可以被认为是人类

---

# 一、前端字符验证码API
是一个免费直接可用的随机字符验证码API,可以用于人机行为验证的一些场景(比如登录场景),且提供了一些参数配置如下

| 序号 | 参数 |值|说明|
|--|--|--|--|
| 1 | CaptchaSize | number |  字体大小(px) |
| 2|CaptchaNoise  | number | 噪声线条数 |
| 3| CaptchaW | number |  svg宽度(px)|
| 4|  CaptchaH| number |   svg高度(px) |
| 5| CaptchaNum | number | 验证字符数量 |
| 6| ColaKey | string | 必填参数,唯一的随机Colakey,官网获取 |



**注意!!!!: 如果您还没有Colakey,请先请前往官网获取
官网地址:[http://luckycola.com.cn/](http://luckycola.com.cn/)**




# 二、使用步骤
## 1.如何获取验证码

***重要提示:建议使用https协议,当https协议无法使用时再尝试使用http协议***
请求下面接口即可

请求方式: GET

```
http(s)://luckycola.com.cn/captcha/getCaptcha?CaptchaSize=44&CaptchaNoise=9&CaptchaW=100&CaptchaH=80$CaptchaNum=5
```

## 2.如何验证验证码
**请求下面接口即可**
请求方式: POST

**参数:** 
| 序号 | 参数 |值|说明|
|--|--|--|--|
| 1 | captcha | string |  用户输入的验证码 |
| 2| ColaKey | string | 必填参数,唯一的随机Colakey,官网获取 |
```c
http(s)://luckycola.com.cn/captcha/validateCaptcha
```

请求响应值code=0为验证成功,否则验证失败

**注意!!!!: 如果您还没有Colakey,请先请前往官网获取
官网地址:[http://luckycola.com.cn/](http://luckycola.com.cn/)**

---


# 三、在线demo


在线demo:[点击查看>>](http://luckycola.com.cn/public/demos/captcha.html)


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>captcha</title>
</head>
<body>
    <div id="app">
        <img  :src="captchaUrl" alt="captcha"
        @click="getCaptcha" ref="captcha">

        <input
            type="text"
            style="display: block;margin: 10px 0 10px 0;"
            placeholder="请输入验证码"
            v-model="message">
            
        <div v-if="errmsg" style="color: red; font-size: 12px; margin-bottom: 10px;">{{errmsg}}</div>
        <button @click="submitFn">提交验证</button>
    </div>


<script src="https://unpkg.com/vue/dist/vue.global.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>
   const { createApp } = Vue
    
    let app = createApp({
      data() {
        return {
          message: '',
          errmsg: '',
          captchaUrl: 'http://luckycola.com.cn/captcha/getCaptcha?CaptchaSize=44&CaptchaNoise=4&CaptchaW=100&CaptchaH=80&CaptchaNum=4&ColaKey=NaTu16773439832446NZkH6'
        //   captchaUrl: 'http://localhost/captcha/getCaptcha?CaptchaSize=44&CaptchaNoise=4&CaptchaW=100&CaptchaH=80&CaptchaNum=4&ColaKey=NaTu16773439832446NZkH6'
        }
      },
      methods: {
        // 获取一个新的图片验证码
        getCaptcha () {
            // 每次指定的src要不一样，img才会重新请求，可以使用Date.now()小技巧
            this.$refs.captcha.src = this.captchaUrl + '&time=' + Date.now()
        },
        submitFn () {
            let me = this;
            if (!this.message) {
                return this.errmsg = '请输入验证码'
            };
            axios.d
            axios({
                url: 'http://luckycola.com.cn/captcha/validateCaptcha', 
                method: 'post',
                withCredentials: true,
                data: {
                    captcha: this.message,
                    ColaKey: 'NaTu16773439832446NZkH6'
                }
            })
                .then(function (res) {
                    // 请求成功返回
                    let resData = res.data;
                    if (resData.code === 0) {
                        alert('验证成功');
                    } else if (resData.code === -99) {
                        alert(resData.msg);
                    } else {
                        alert('验证码错误,验证失败');
                    }
                    console.log(res);
                    me.getCaptcha();
                })
                .catch(function (err) {
                    // 请求失败返回
                    console.log(err);
                    alert('请求失败');
                })
        }
      },
    }).mount('#app');
    app.$watch('message', function(val) {
        console.log('message change=>', val);
    })
</script>

</body>
</html>

```

**注意: 请求时需要允许携带cookie!!!**
