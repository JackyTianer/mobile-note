#Mobile开发笔记
## 理解viewport
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0,minimum-scale=1,s user-scalable=0;" />
```
在*mobile*开发中,我们经常看见这样的一段话,这句话简单的说,就是为了保证移动配置页不会出现缩放,横向滚动条等现象,我们来具体看看这些设定为何起到作用.其实,他
起到的是控制*mobile*中 <html>这个跟标签的作用,注意,只有 *mobile*, web是无效的.
- width:控制html的宽度  举个例子,如果你没有写这个<meta>标签,那么你在dody下一个子标签设置宽度为10%,会有什么效果?在web下显而易见,10%对应*浏览器*宽度的大小,浏览器越宽,那么
  这个元素展现的宽度越大.移动端呢?也是这样吗,答案否定,移动端如果没有设置了*width*的meta标签,那么,一般手机10%的宽度,对应的实际大小为98px,也就是说Mobile中
  默认html的宽度为980px(**大部分手机**), 这时候如果你设置了width=640,那么 10%的宽度就对应的是64px;
- initial-scale,maximum-scale, minimum-scale, 设置页面缩放.

### 常用的布局方式

1. 百分比布局, 将设计稿的宽度换算成百分比，也就是说宽度随着不同的device-width，肯定会有所变化，这样做简单方便,但是.
    - 当你给父标签设置了宽度,子标签的百分比不是宽度的百分比,这里容易出现问题.
    - 不方便做正方形
    
2. 设置 width=640, 设置缩放比例为0.5,一般视觉稿给的图像宽度都为640,使用这种方式的好处就是可以完全的使用px来布局，间距，宽高可以很好的适应，不过带来的问题就是在iphone6以上以及android机中device-width不是320造成的间距问题，这时候可以动态计算缩放比例来解决。

3. rem布局, 字体宽度等使用rem来布局，因为方便计算并且又满足chrome字体最小值12px的要求，将html的font-size设置成100px,根据视觉稿将px转换成rem来布局，并且，可以通过媒体查询设置不同device-width宽度的跟字体大小值，如，视觉稿是640宽度，你要兼容iphone6的宽度的话， 使用 (375 * 2) * 100/640 计算即将device-width宽度为375的机型设置rem为117.1875
缺点也很明显
    - 对茫茫多的android机型难以做到完全兼容，奇葩宽度在android会出现
    - 视觉如果将常用设计宽度更改了的话，不能统一用一个根font-size
    
4. 动态rem布局,参考淘宝主页， 使用动态计算rem的方式，实时改变 html的font-size，并且通过dpr来动态改变缩放比例
```javascript
    (function (doc, win) {
              var docEle = doc.documentElement,
                  isIos = navigator.userAgent.match(/iphone|ipod|ipad/gi),
                  dpr=Math.min(win.devicePixelRatio, 3);
                  scale = 1 / dpr,
    
                  resizeEvent = 'orientationchange' in window ? 'orientationchange' : 'resize';
    
              docEle.dataset.dpr = dpr;
    
              var metaEle = doc.createElement('meta');
              metaEle.name = 'viewport';
              metaEle.content = 'initial-scale=' + scale + ',maximum-scale=' + scale;
              docEle.firstElementChild.appendChild(metaEle);
              
    
              var recalCulate = function () {
                      var width = docEle.clientWidth;
                      if (width / dpr > 640) {
                          width = 640 * dpr;
                       }
                    docEle.style.fontSize = 100 * (width / 750) + 'px';
                };
    
              recalCulate()
    
              if (!doc.addEventListener) return;
              win.addEventListener(resizeEvent, recalCulate, false);
            })(document, window);
```
上面代码做了
+ 获取设备dpr(即手机分辨率/device-widht);
+ 算出缩放比例 scale = 1/dpr;
+ 创建meta 并将scale赋予initial-scale，maximum-scale;
+ 插入文档
+ 获取手机真实宽度，以750宽度100px为基准，算出其他屏幕宽度的基准font-size
+ 监听屏幕变化，响应

我们发现，在这种方式里面，并没有主动去设置width=device-width,而是利用了dpr计算缩放比例，通过缩放的方式，让屏幕不再出现横向滚动条。
如果使用这种方式，可以比较完美的解决手机兼容问题, 不过这样做和上面rem布局也有一个缺点，就是无时无刻需要换算rem和px，不过这可以在sass中写一个函数来解决

```sass
@function px2rem( $px ){
    @return $px*750/$designWidth/100 + rem; }
$designWidth : 750; 
```