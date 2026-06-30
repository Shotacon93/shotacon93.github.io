
> 食用方法可以是img标签嵌入在网页中, 也可以在iwall.app里面设置你的桌面背景.
> 还有其他好用的API吗? 请在下放留下您的评论. 非常感谢!
> 更新时间: 2022年10月20日

1. 速度: ★★★★★
   功能: 返回随机图片, 可以指定尺寸(宽高), 通过302转发展示.
   地址: https://picsum.photos/200/300

2. 速度: ★★★★★
   功能: 返回指定数量的狗狗图片, count=1-100, urls=是否返回完整连接, httpsUrls=是否https
   地址: https://shibe.online/api/shibes?count=10&urls=true&httpsUrls=true

3. 速度: ★★★★★
	功能: 自适应随机图片, category支持风景(fengjing), 动漫(dongman), 必应(biying).
			 px分辨率支持电脑(pc), 手机(m). 
	地址: `https://tuapi.eees.cc/api.php?category=dongman&type=302&px=pc`

4. 速度: ★★★★★

   ~~功能: 返回Bing的随机图片~~(已失效)

   ~~地址: https://uploadbeta.com/api/pictures/random/?key=BingEverydayWallpaperPicture~~ 
   <!-- more -->

5. 速度: ★★★★★

   功能: 返回随机图片, 需指定分辨率, 如果指定的分辨率没有找到图片则返回空页面

   ~~地址: http://lorempixel.com/1600/900~~ (实测不能用了, 好像是在卖域名)

6. 速度: ★★★★☆

   功能: 来自github的项目 https://github.com/xCss/bing
   ~~地址: https://bing.ioliu.cn/v1/rand (返回随机图片)~~ 

   因为作者的问题, 无法通过链接直接获取随机图片, 只能通过链接获取到json, 地址如下:
   https://bing.ioliu.cn/v1/rand?type=json


   ```
   {
       "data":{
           "enddate":"20180212",
           "url":"http://www.bing.com/az/hprichbg/rb/FrozenWaterfallJasper_EN-CA10140771944_1920x1080.jpg",
           "bmiddle_pic":null,
           "original_pic":null,
           "thumbnail_pic":null
       },
       "status":{
           "code":200,
           "message":""
       }
   }
   ```


   指定大小亲测也不好使了...
   ~~如需指定大小: https://bing.ioliu.cn/v1/rand?w=1920&h=1200~~   

   ```
   /**
    * 已知分辨率
    */
   resolutions: [
       '1920x1200',
       '1920x1080',
       '1366x768',
       '1280x768',
       '1024x768',
       '800x600',
       '800x480',
       '768x1280',
       '720x1280',
       '640x480',
       '480x800',
       '400x240',
       '320x240',
       '240x320'
   ]
   ```

7. 速度: ★★★★☆

   功能: 返回指定分辨率的随机图片, (加载最慢, 可能是因为服务器在国外)
   地址: https://unsplash.it/1600/900?random 

8. ~~速度: ★★★★★~~   (亲测失效...)

   ~~功能: 返回随机的二次元图片, 国人之作, 发现地址: https://www.dujin.org/12142.html/amp~~ 
   ~~地址: https://api.xjdog.cn/Get-Image~~ 

9. 速度: ★★★★★★   (还有效, 但每天一张比较尴尬)

   功能: 返回随机Bing图片, 其中数字为分辨率, 例如 1366x768 -> 1366,  1920x1080 -> 1920
   地址: https://api.dujin.org/bing/1366.php