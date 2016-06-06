#Mobile开发笔记
## 理解viewport
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0,minimum-scale=1,s user-scalable=0;" />
```
在*mobile*开发中,我们经常看见这样的一段话,这句话简单的说,就是为了保证移动配置页不会出现缩放,横向滚动条等现象,我们来具体看看这些设定为何起到作用.其实,他
起到的是控制*mobile*中 <html>这个跟标签的作用,注意,只有 *mobile*, web是无效的.
- width
    1.举个例子,如果你没有写这个<meta>标签,那么
```javascript
    
```