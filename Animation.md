---
title: 'Animation'
date: 2016-01-17 21:38:50
tags:
---
# CA:CoreAnimation动画
>属于CALayer层的动画,属于最底层,UIView的底层也是CALayer,UIView是基于CALayer的封装,CALayer具备UIView的所有属性和方法

### 1. CABasicAnimation

```objc
  CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"transform.rotation.y"];
  animation.toValue = @(M_PI*2);
  animation.duration = 3;
  animation.repeatCount = MAXFLOAT;
  [_imageView.layer addAnimation:animation forKey:@"animationY"];  
```
1. **key**：addAnimation时的key,为自定义动画时的标识，可用于指定动画进行操作,比如删除指定动画
   key获取
2. **KeyPath**：系统动画类型标识，用于获得系统动画
  - transform.rotation.x :系统绕x轴旋转动画
  - transform.rotation.y :系统绕y轴旋转动画
  - transform.rotation.z :系统绕z轴旋转动画
  - position : 用于CAKeyframeAnimation做沿“轨迹”运动,见下详细示例

### 2. *CAKeyframeAnimation*
  >CA帧动画,通常可用于按“轨迹”运动，与UIBezierPath结合

  ```objc
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:rect];
    UIBezierPath *arc = [UIBezierPath bezierPathWithArcCenter:self.view.center radius:120 startAngle:M_PI_2*3 endAngle:M_PI_2*3+M_PI*2 clockwise:YES];
      //要沿着路劲跑，必须用关键帧动画
      //position:沿着路径动画
    CAKeyframeAnimation *moveAnimation = [CAKeyframeAnimation animationWithKeyPath:@"position"];
      //设置动画的路径
    moveAnimation.path = arc.CGPath;
    moveAnimation.duration = 3;
    moveAnimation.repeatCount = MAXFLOAT;
    [_imageView.layer addAnimation:moveAnimation forKey:@"runWithPath"];
  ```
<!--more-->

### 3. CGAffineTransfor

### 4. CAAnimationGroup
>动画组,可同时为一个对象添加管理多个动画,

```objc
CAAnimationGroup *group =[CAAnimationGroup animation];
  group.animations = @[xScaleAnimation,yScaleAnimation,moveAnimation];
  group.repeatCount = 2;
  group.duration = 2;
   //可以通过代理来监听动画的开始和结束，此协议已经被自动引入，不需要手动再引
group.delegate = self;
[_imageView.layer addAnimation:group forKey:@"groupAnimation"];
```

### 5. delegateMethod
>监测动画的代理方法,由于CAAnimation自动引入了代理，无需在类名后手动引入代理，只需给代理赋值即可`animation.delegate = self;`

```objc
-(void)animationDidStart:(CAAnimation *)anim{
}
-(void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag{
}
```

# UIView动画
>UIView的Animation是核心动画CoreAnimation对UIView的扩展类

### 1. 基础动画

```objc
  [UIView animateWithDuration:duration
           animations:^{
  }];

  [UIView animateWithDuration:duration
           animations:^{
         } completion:^(BOOL finished) {
  }];
```

### 2. 基础动画，带option选项

```objc
[UIView animateWithDuration:duration
         delay:0
         options:UIViewAnimationOptionCurveLinear
         animations:^{
      } completion:^(BOOL finished){
}];
  ```
其中的**option**参数有以下几种：
```Objective-C
- UIViewAnimationOptionCurveLinear(线性变化)
- UIViewAnimationOptionRepeat（重复）
- UIViewAnimationOptionAutoreverse（自动重来）
```

### 3. 带有阻尼damping值,加速度velocity的动画(可实现弹跳动画)

```objc
[UIView animateWithDuration:duration
         delay:0
         usingSpringWithDamping:damping
         initialSpringVelocity:velocity
         options:UIViewAnimationOptionCurveLinear animations:^{
          //动画进行时操作
       } completion:^(BOOL finished) {
          //完成操作
}];
```
  1. **damping** :值越小，弹跳次数越多
  2. **velocity** :  值越大，弹跳速度越快

### 4. 帧动画,可对动画过程中的某个阶段[帧]进行操作

```objc
[UIView animateKeyframesWithDuration:duration
         delay:0
         options:UIViewKeyframeAnimationOptionCalculationModeLinear
         animations:^{
            //添加帧动画
                [UIView addKeyframeWithRelativeStartTime:start
                        relativeDuration:duratuion
                        animations:^{
                }];
            //如果用Autolayout做布局，需调用
                [self.view layoutIfNeeded];
      } completion:^(BOOL finished) {
            //完成操作
}];
```

  1. **添加帧动画**

    其中*start*,*duration*参数为总时段的所占比率，如start=1/3,duration=1/3,即表示从时段的第1/3阶段开始,进行1/3时长的动画
  2. **加载底层视图**

   以下形式为**单独刷新imageView的layout**，如在帧中,分时控制，一个物体自上而下,自左而右碰撞(不断走Z字路线),分段里,需单独改变,用以下方式.此时如果用Masnory做AutoLayout,需注意用mas_remakeConstraints:^(MASConstraintMaker *make)
  3. **layoutIfNeeded**

    当使用Autuolayout做布局时,需要调用*layoutIfNeeded*刷新视图,当只需要刷新特定对象时,需要首先*setNeedsLayout*,如下图示例所示
```objc
[self.imageView mas_remakeConstraints:^(MASConstraintMaker *make) {
               make.right.equalTo(0);
               make.bottom.equalTo(-200);
               make.size.equalTo(CGSizeMake(100, 100));
           }];
[self.imageView setNeedsLayout];
[self.imageView layoutIfNeeded];
```
