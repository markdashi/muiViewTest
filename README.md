# muiViewTest

The Project is based mui that can test mui.view.js use.

依赖

- [x] mui.js
- [x] mui.view.js
- [x] mui.css

# html结构

## 主体部分
```
<!--页面主结构开始-->
<div id="app" class="mui-views">
 <div class="mui-view">
	<div class="mui-navbar">
	</div>
   <div class="mui-pages">
   </div>
 </div>
</div>
<!--页面主结构结束-->
```
.mui-view->.mui-navbar用于加载单页的header部分
.mui-view->.mui-mui-pages用于加载单页的content部分

# 单页结构
```
<div id="setting" class="mui-page">
		<div class="mui-navbar-inner mui-bar mui-bar-nav">
			<h1 class="mui-center mui-title">设置</h1>
		</div>
		<ul class="mui-table-view">
	        <li class="mui-table-view-cell">
	            <a class="mui-navigate-right" href="#accout">
	                账号
	            </a>
	        </li>
	        <li class="mui-table-view-cell">
	            <a class="mui-navigate-right" href="#about">
	                关于
	            </a>
	        </li>
		  </ul>
	</div>
    <div id="accout" class="mui-page">
    	<div class="mui-navbar-inner mui-bar mui-bar-nav">
		<button type="button" class="mui-left mui-action-back mui-btn  mui-btn-link mui-btn-nav mui-pull-left">
			<span class="mui-icon mui-icon-left-nav"></span>设置
		</button>
		<h1 class="mui-center mui-title">账号</h1>
	</div>
    </div>
    <div id="about" class="mui-page">
    	    	<div class="mui-navbar-inner mui-bar mui-bar-nav">
		<button type="button" class="mui-left mui-action-back mui-btn  mui-btn-link mui-btn-nav mui-pull-left">
			<span class="mui-icon mui-icon-left-nav"></span>
		</button>
		<h1 class="mui-center mui-title">关于</h1>
	</div>
    </div>
```
每个单页的内容区域，都在.mui-page容器中。
header部分，必须按照.mui-page->.mui-navbar-inner的结构构建。内容区域，推荐使用.mui-page.content。

# css部分
```
.mui-views,
			.mui-view,
			.mui-pages,
			.mui-page,
			.mui-page-content {
				position: absolute;
				left: 0;
				right: 0;
				top: 0;
				bottom: 0;
				width: 100%;
				height: 100%;
				background-color: #efeff4;
			}
			
			.mui-pages {
				top: 46px;
				height: auto;
			}
			
			.mui-page.mui-transitioning {
				-webkit-transition: -webkit-transform 300ms ease;
				transition: transform 300ms ease;
			}
			
			.mui-page-left {
				-webkit-transform: translate3d(0, 0, 0);
				transform: translate3d(0, 0, 0);
			}
			
			.mui-ios .mui-page-left {
				-webkit-transform: translate3d(-20%, 0, 0);
				transform: translate3d(-20%, 0, 0);
			}
			
			.mui-navbar .mui-btn-nav {
				-webkit-transition: none;
				transition: none;
				-webkit-transition-duration: .0s;
				transition-duration: .0s;
			}
			
			.mui-navbar .mui-bar .mui-title {
				display: inline-block;
				width: auto;
			}
			/*阴影*/
			.mui-page-shadow {
				position: absolute;
				right: 100%;
				top: 0;
				width: 16px;
				height: 100%;
				z-index: -1;
				content: '';
			}
			/*阴影*/
			
			.mui-page-shadow {
				background: -webkit-linear-gradient(left, rgba(0, 0, 0, 0) 0, rgba(0, 0, 0, 0) 10%, rgba(0, 0, 0, .01) 50%, rgba(0, 0, 0, .2) 100%);
				background: linear-gradient(to right, rgba(0, 0, 0, 0) 0, rgba(0, 0, 0, 0) 10%, rgba(0, 0, 0, .01) 50%, rgba(0, 0, 0, .2) 100%);
			}
			/*切换动画*/
			
			.mui-navbar-inner.mui-transitioning,
			.mui-navbar-inner .mui-transitioning {
				-webkit-transition: opacity 300ms ease, -webkit-transform 300ms ease;
				transition: opacity 300ms ease, transform 300ms ease;
			}
			.mui-page {
				display: none;
			}
			
			.mui-pages .mui-page {
				display: block;
			}
```

# javascript部分

### 初始化插件
```
var oldBack = mui.back;
	mui.back = function() {
		if(viewApi.canBack()) { //如果view可以后退，则执行view的后退
			viewApi.back();
		} else { //执行webview后退
			oldBack();
		}
	};
```

### 切换页面
两种方式可以实现页面切换
- 锚点方式
```
<!--锚点部分-->
<a href="#pageid"></a>
<!--单页部分-->
<div class="mui-page" id="pageid"></div>
```
- js控制

```
var elem = document.querySelector('a');
elem.addEventListener('tap', function(){
    viewApi.go('#pageid');
});

```
### 切换事件

监听页面切换事件方案，通过view元素监听所有页面切换事件，
目前提供
- pageBeforeShow
- pageShow
- pageBeforeBack
- pageBack四种事件(before事件为动画开始前触发)
 第一个参数为事件名称，第二个参数为事件回调，其中e.detail.page为当前页面的html对象 
```
view.addEventListener('pageBeforeShow', function(e) {
		var page = e.detail.page;
		//console.log('page:' + page);
		console.log(e.detail.page.id + ' beforeShow');
	});
	view.addEventListener('pageShow', function(e) {
		console.log(e.detail.page.id + ' show');
	});
	view.addEventListener('pageBeforeBack', function(e) {
		console.log(e.detail.page.id + ' beforeBack');
	});
	view.addEventListener('pageBack', function(e) {
		console.log(e.detail.page.id + ' back');
	});
```

### 后退操作
```
var oldBack = mui.back;
mui.back = function() {
    if(viewApi.canBack()) {
        viewApi.back();
    } else {
        oldBack();
    }
};
```