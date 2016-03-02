---
title: 绘图
date: 2016-02-07 09:35:22
tags:
---
# 绘图
### （一）UIBezierPath

-画出坠落轨迹

```objc
- (void)drawRect:(CGRect)rect {
    //draw a parabola with one point
    UIBezierPath *parabola_path = [UIBezierPath bezierPath];
    [parabola_path moveToPoint:CGPointMake(150, 40)];
    //加一个受力点
    [parabola_path addQuadCurveToPoint:CGPointMake(150, 140) controlPoint:CGPointMake(220, 65)];
    [parabola_path moveToPoint:CGPointMake(220, 65)];
    //[parabola_path addLineToPoint:CGPointMake(200, 65)];
     [[UIColor orangeColor]setStroke];
    parabola_path.lineWidth = 10;
    [parabola_path stroke];  
    //draw a parabol with more than one point
    UIBezierPath *sline_path = [UIBezierPath bezierPath];
    [sline_path moveToPoint:CGPointMake(150, 30)];
    //加两个受力点
    [sline_path addCurveToPoint:CGPointMake(150, 400) controlPoint1:CGPointMake(80,70) controlPoint2:CGPointMake(230, 160)];
    sline_path.lineWidth = 5;
    [sline_path stroke];
    //sline_path
}
```
<!--more-->

-在draw:rect方法外画图（比如控制器中）

```objc
- (void)drawRect:(CGRect)rect {
    // Drawing code
    CGContextRef context = UIGraphicsGetCurrentContext();
    CGContextMoveToPoint(context, 40, 40);
    CGContextAddLineToPoint(context, 40, 140);
    CGContextAddLineToPoint(context, 140, 40);
    CGContextAddLineToPoint(context, 40, 40);
    // set the color of context ,stroke
    // what is CGColor ?
    CGContextSetStrokeColorWithColor(context, [UIColor redColor].CGColor);
    CGContextSetFillColorWithColor(context, [UIColor greenColor].CGColor);

    //stroke path(miao bian) and fill path(tian chong) at the same time
    CGContextDrawPath(context, kCGPathFillStroke);
}
```
- 画出字符

```objc
- (void)drawRect:(CGRect)rect {
  NSString *temp = @"boy,good good study,day day up,孩子，孩子，你为什么这么坏，脏话，下流话，你说出来，啊啊啊，what are you saying";
  //存放字符串与绘画有关属性的字典
  NSDictionary *dic = @{NSFontAttributeName:[UIFont systemFontOfSize:20],NSForegroundColorAttributeName:[UIColor blackColor]};
  //根据字符串的属性，和最大的宽度，高度，算出字符串显示范围
  //限定最宽200，算一下要多高能显示全
  //options:根据字体，最后一行课件，行间距
  CGRect stRect = [temp boundingRectWithSize:CGSizeMake(200, MAXFLOAT) options:NSStringDrawingTruncatesLastVisibleLine|NSStringDrawingUsesLineFragmentOrigin|NSStringDrawingTruncatesLastVisibleLine attributes:dic context:nil];
  //画出字符串
  [temp drawInRect:CGRectMake(20, 20, 200, stRect.size.height) withAttributes:dic];
}
```
- 画出图片

```objc
- (void)drawRect:(CGRect)rect {

    UIImage *image = [UIImage imageNamed:@"welcome_320x480_1"];
    //起点:70 80 大小206 250
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(70, 80, 206, 250)];
    //剪切属性，让路径的外部不可以绘画
    [path addClip];
    [image drawInRect:CGRectMake(0, 0, 320,480)];

}
```

### （二）Core Graphics

```objc
- (void)drawRect:(CGRect)rect {
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(40, 40)];
    [path addLineToPoint:CGPointMake(40, 140)];
    [path addLineToPoint:CGPointMake(140, 40)];
    [path addLineToPoint:CGPointMake(40, 40)];

    //set the color of strokepath
    //set the color of the fillpath
    [[UIColor redColor]setStroke];
    [[UIColor greenColor]setFill];

    //set the width of the line
    path.lineWidth = 10;
    //set the style of the jionline(lianjie chu yangshi)
    path.lineJoinStyle = kCGLineJoinRound;
    //set the corner of line(xian tou yangshi)
    path.lineCapStyle = kCGLineCapRound;

    [path stroke];
    [path fill];
}
```
