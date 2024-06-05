---
title: Java ImageIO 图片处理后变红 解决办法记录
categories:
  - 开发
tags:
  - java
date: 2020-12-07 23:20:07.610000
---
- 原因：根据网上搜罗的一大堆文章以及自己的发现，是因为原始图片（jpeg）带有alpha通道才会变红，在mac上直接显示简介的看到。


![image.png](http://image.75051685.xyz/blog/image_1607354092161.png)
- 然后发现使用下面这个方式可以解决变红的问题

```
// 把这行换成下面的方式
BufferedImage image = ImageIO.read(originFile);

// 这里是直接根据url读取图片
public static BufferedImage getBufferedImage(String  imgUrl) throws MalformedURLException {
    URL url = new URL(imgUrl);
    ImageIcon icon = new ImageIcon(url);
    Image image = icon.getImage();
	
	// 如果是从本地加载，就用这种方式，没亲自测试过
	// Image src=Toolkit.getDefaultToolkit().getImage(filePath);

    // This code ensures that all the pixels in the image are loaded
    BufferedImage bimage = null;
    GraphicsEnvironment ge = GraphicsEnvironment
            .getLocalGraphicsEnvironment();
    try {
        int transparency = Transparency.OPAQUE;
        GraphicsDevice gs = ge.getDefaultScreenDevice();
        GraphicsConfiguration gc = gs.getDefaultConfiguration();
        bimage = gc.createCompatibleImage(image.getWidth(null),
                image.getHeight(null), transparency);
    } catch (HeadlessException e) {
        // The system does not have a screen
    }
    if (bimage == null) {
        // Create a buffered image using the default color model
        int type = BufferedImage.TYPE_INT_RGB;
        bimage = new BufferedImage(image.getWidth(null),
                image.getHeight(null), type);
    }
    // Copy image to buffered image
    Graphics g = bimage.createGraphics();
    // Paint the image onto the buffered image
    g.drawImage(image, 0, 0, null);
    g.dispose();
    return bimage;
}

```