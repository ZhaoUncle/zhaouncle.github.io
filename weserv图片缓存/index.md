# Weserv图片缓存


<!--more-->

# 1. 介绍

images.weserv.nl 具有全球、缓存、调整大小、开源的属性。主要调用 Cloudfare 的 cdn 来实现图片缓存

- 支持的图片格式有JPEG, PNG, BMP, GIF, TIFF, WebP, PDF, SVG, WebP, GIF
- 支持 IPV6
- 可以使用  https://images.weserv.nl/ 直接调用 http图片缓存，用于通过TLS / SSL进行安全连接

# 2. 使用

该工具最大的地方是可以直接修改图片的大小，[快速参考](https://images.weserv.nl/docs/quick-reference.html)

## 2.1 尺寸

```html
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&w=300&h=300&dpr=2">
```

>w：设置图像的宽度，以像素为单位。
>
>h：设置图像的高度，以像素为单位。
>
>dpr：设备像素比，level 1-8.

## 2.2 自调

```html
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&w=300&h=300&fit=inside">
```

> 默认。保留宽高比，将图像调整为尽可能大的尺寸，同时确保其尺寸小于或等于指定的尺寸。

## 2.3 位置

```html
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&w=300&h=300&fit=cover&a=centor">
```

- `center`：默认

## 2.4 形状

```html
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&w=300&h=300&fit=cover&mask=circle">
```

效果：

- `circle`：圆圈
- `ellipse`
- `triangle`
- `triangle-180`：三角形倒置倾斜
- `pentagon`
- `pentagon-180`：五角大楼倒置倾斜
- `hexagon`
- `square`：方形倾斜45度
- `star`：5点星
- `heart`

## 2.5 方向

```html
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&h=300&flip">
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&h=300&flop">
<img src="//images.weserv.nl/?url=images.weserv.nl/lichtenstein.jpg&h=300&ro=45">
```

> flip: 180°翻转
>
> flop：沿 X 轴翻转
>
> ro：沿定制角度翻转

## 2.6 时长

```html
//images.weserv.nl?url=images.weserv.nl/lichtenstein.jpg&w=100&maxage=31d
```

> maxage: 缓存控制时长，
>
> - `d`： 天
> - `y`：年，365天

## 2.7 默认图片

```html
<img src="//images.weserv.nl/?url=example.org/noimage.jpg&default=ssl:images.weserv.nl%2F%3Furl%3Dimages.weserv.nl/lichtenstein.jpg%26w%3D300">

https://images.weserv.nl/?url=https://wfqqreader-1252317822.image.myqcloud.com/cover/71/25066071/t6_25066071.jpg&default=ssl:images.weserv.nl/?url=https://img9.doubanio.com/view/subject/l/public/s26872396.jpg
```

如果加载图像时出现问题，则显示错误。但是，可能需要将默认图像传递给用户，而不是向用户提供损坏的图像。

该URL不得包含`default`查询字符串（如果包含，则将被忽略）。





# 3. 参考资料：

https://github.com/weserv/images

https://images.weserv.nl/


