# 微信小程序图片裁剪：

# Bug1：

裁剪框锁定在左上角的bug：

![image-20231011114633327](/Users/wzy/Library/Application Support/typora-user-images/image-20231011114633327.png)

![IMG_8915](/Users/wzy/Downloads/IMG_8915.jpg)

在模拟器上e.touches[0]的参数存在pageX和pageY，而在真机中不存在这两个属性

# Bug2：

渲染小图渲染不出来或者卡顿

这是渲染小图（预览图）的方法：

```typescript
 drawSmallImg() {

    const {x,y,size} = this.data

    this.clipCtx.drawImage(this.cvs,x,y,size,size,0,0,size,size )

    // const imgData = this.ctx.getImageData(x, y, size, size);

    // this.clipCtx.putImageData(imgData, 0, 0);

 },
```

iOS系统drawImage传入HTMLCanvasElement类型无法显示，经过微信社区查找资料得知，这是一个iOS端的bug，经过测试模拟器和安卓都运行成功

https://developers.weixin.qq.com/community/develop/doc/000ec68c7bc498740ccf7dee656000?jumpto=comment

https://developers.weixin.qq.com/community/develop/doc/0008aec945ce30fab40030ee56b800

但是如果用getImageData和.putImageData方法来实时渲染的话，结果上看可以达成的，不过这两个方法在小程序上运行一次大概在20毫秒左右，运行起来很卡顿。



# 改良：

现在的图片不再铺满全屏，侧面留有空间以供用户进行页面滚动操作
