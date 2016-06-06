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
    > 当你给父标签设置了宽度,子标签的百分比不是宽度的百分比,这里容易出现问题
    > 不方便做正方形
    
2. 设置 width=640, 设置缩放比例为0.5,一般视觉稿给的图像宽度都为640,使用这种方式的好处就是可以完全的使用px来布局，间距，宽高可以很好的适应，不过带来的问题就是在iphone6以上以及android机中device-width不是320造成的间距问题，这时候可以动态计算缩放比例来解决。

```javascript
    
```