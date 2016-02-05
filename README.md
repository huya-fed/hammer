
# 移动端手势库hammerJS 2.0.6
	
> hammerJS是一个优秀的、轻量级的触屏设备手势库、允许同时监听多个手势、自定义识别器，也可以识别滑动方向。
> http://www.cnblogs.com/vajoy/p/4011723.html
> 
> http://hammerjs.github.io/api/

----------


## 开始

HammerJS是一个开源的库，可以识别由 touch, mouse 和 pointerEvents 触发的系列手势。它非常小巧，压缩后仅有3.96kb，并没有多余的脚本依赖。

> 2.0版本的变动：彻底重写了源码，包括可复用的识别器（recognizer）、提升了对最新移动端浏览器可用的触摸行为css属性的支持，另支持了多个hammer实例同时使用，让多用户同时使用一台设备也不在话下。



## 使用

HammerJS的使用方式非常简单，只要将库引入到文件中，并创建一个新的实例即可：

	var hammertime = new Hammer(myElement, myOptions);
	hammertime.on('pan', function(ev) {
	    console.log(ev);
	});

它会默认为这个对象添加一系列识别器，包括 tap<点>, doubletap<双点击>, press<按住>, 水平方位的pan<平移> 和 swipe<快速滑动>, 以及多触点的 pinch<捏放> 和 rotate<旋转>识别器。不过呢，其中的 pinch 和 rotate 默认是不可用的，因为它们可能会导致元素被卡住，如果你想启用它们，可以加上这两句：

	hammertime.get('pinch').set({ enable: true });
	hammertime.get('rotate').set({ enable: true });

若要允许识别器识别垂直方位或全部方位的 pan 和 swipe，可以这么写：

	hammertime.get('pan').set({ direction: Hammer.DIRECTION_ALL });
	hammertime.get('swipe').set({ direction: Hammer.DIRECTION_VERTICAL });

另建议加上如下meta标签，防止doubletap 或 pinch 缩放了viewport：

	<meta name="viewport" content="user-scalable=no, width=device-width, initial-scale=1, maximum-scale=1">

## 更多控制

你可以为你的实例设置属于你自己的识别器，虽然要多写一点代码，但能让你控制更多能被识别的手势：


	var mc = new Hammer.Manager(myElement, myOptions);
	
	mc.add( new Hammer.Pan({ direction: Hammer.DIRECTION_ALL, threshold: 0 }) );
	mc.add( new Hammer.Tap({ event: 'quadrupletap', taps: 4 }) );
	
	mc.on("pan", handlePan);
	mc.on("quadrupletap", handleTaps);


上述的代码创建了一个实例（mc），它包含了一个 pan 和一个 quadrupletap 手势，识别器实例会在它们被添加（add）之后就不断地执行，且（一个识别器实例）只能识别一个（手势）。


## 贴士和窍门

### 1. 试着避免垂直方向上的 pan/swipe

垂直方向上的平移操作一般是用来滚动你的页面的，而且有些（过时的）浏览器不会传递事件，导致Hammer无法识别这些手势。你可以尝试换另一种可替换的途径来实现相同的动作。

### 2. 在设备上做测试

有时候Hammer需要做一些调整，像swipe的速率或其它阈值，如果你在一台反应较慢的设备上做测试，那么你要保证你的回调越简单越好。有些站点例如JankFree.org上有专门的文章来介绍如何提升展示效果。


### 3. 去掉Windows Phone点击时的高亮效果

你在Windows Phone上的IE10和IE11里tap某元素时，会有一个小小的tap高亮效果，加上这个meta标签可以取消这种效果

	<meta name="msapplication-tap-highlight" content="no" />


### 4. "我怎么选择不了文本了！"

Hammer设置了一个属性用来提升桌面平移操作的用户体验（UX）。常规来说，当你在桌面级浏览器上拖动页面时，你应该是可以正常选中文本的，但user-select这个CSS属性禁用了这功能。
如果你在意文本选择功能，同时觉得桌面级的体验没必要太尽善尽美，你可以很轻松地取消这个默认选项——确保在创建实例之前执行：

	delete Hammer.defaults.cssProps.userSelect;



## 常规API


























