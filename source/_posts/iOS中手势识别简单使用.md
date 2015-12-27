title: iOS中手势识别简单使用
date: 2015-12-28 00:21:05
categories: iOS
tags: [OC, 手势识别]
---

> 利用UIGestureRecognizer, 能轻松识别用户在某个view上面做的一些常见手势。

<!--more-->

* UIGestureRecognizer是一个抽象类, 定义了所有手势的基本行为, 使用它的子类才能处理具体的手势
	
	* UITapGestureRecognizer(点击)
	* UIRotationGestureRecognizer(旋转)
	* UIPinchGestureRecognizer(捏合, 用于缩放)
	* UIPanGestureRecognize(拖拽)
	* UISwipeGestureRecognizer(轻扫)
	* UILongPressGestureRecognizer(长按)

### 手势识别器用法

#### UITapGestureRecognizer(点击)

	UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] init];
	
	//设置手势识别器对应的具体属性
	//连续点击2次
	tap.numberOfTapsRequired = 2;
	//设置需要2根手指一起点击
	tap.numberOfTouchesRequired = 2;
	
	//添加手势识别器到对象的view上
	[self.view addGestureRecognizer:tap];
	
	//监听手势的触发
	[tap addTarget:self action:@selector(tap:)];
	
####  UIRotationGestureRecognizer(旋转)
	
	UIRotationGestureRecognizer *rotation = [[UIRotationGestureRecognizer alloc] initWithTarget:self action:@selector(rota:)];
    
    [self.view addGestureRecognizer:rotation];
    
监听函数:

	- (void) rota: (UIRotationGestureRecognizer *) rot
	{
	    self.view.transform =  CGAffineTransformRotate(self.view.transform, rot。rotation);
  		
  		//复位
    	rot.rotation = 0;
	}
	
#### 注意 ####

---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上面旋转之后要进行复位, 因为手势的取值都是相对最原始的位置，我们应该是需要相对上一次，因此每次调用，就复位一下，每次都是从零开始旋转角度。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;例如第一次旋转了20(rot.rotation为20),第二次旋转了30(rot.rotation的值为上次的值加上30, 即50), 因此第二次旋转了70。因此第一次旋转后应该把rot.rotation设置为0。

----


	
#### UIPinchGestureRecognizer(捏合, 用于缩放)

	UIPinchGestureRecognizer *pin = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(pin:)];
	
    [self.view addGestureRecognizer:pin];

pin函数:

	- (void) pin: (UIPinchGestureRecognizer *) pin
	{
    	self.view.transform = CGAffineTransformScale(self.view.transform, pin.scale, pin.scale);
    
    	//和上面一样要进行复位
    	pin.scale = 1;
	}

#### UIPanGestureRecognize(拖拽)
	
	UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];
	
    [self.view addGestureRecognizer:pan];

监听函数: 

	- (void) pan: (UIPanGestureRecognizer *) pan
	{
   		CGPoint curP = [pan translationInView:self.view];
    
    	self.view.transform = CGAffineTransformTranslate(self.view.transform, curP.x, curP.y);
    
    	[pan setTranslation:CGPointZero inView:self.view];
	}
	
#### UISwipeGestureRecognizer(轻扫)

	UISwipeGestureRecognizer *swipe = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipe:)];
	//设置轻扫的方向, 默认为从左到右, 即不会处理其他方向的轻扫
	//如果要处理多个方向轻扫, 应该多增加手势识别器
    swipe.direction = UISwipeGestureRecognizerDirectionLeft;
    [self.view addGestureRecognizer:swipe];
    
#### UILongPressGestureRecognizer(长按)

	UILongPressGestureRecognizer *longPre = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longpre:)];
    
    [self.view addGestureRecognizer:longPre];
    
默认情况下不支持多个手势, 例如同时进行旋转和缩放, 如果要支持多个手势, 应该实现代理方法:

	- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer
	{
    	return YES;
	}
