# iscroll5 多选项卡+上拉刷新+下拉加载

## HTML
```html
<div id="tabs">
	<div class="tab-scroller">
		<a class="active" href="#">选项卡1</a>
		<a href="#">选项卡2</a>
		<a href="#">选项卡3</a>
		<a href="#">选项卡4</a>
		<a href="#">选项卡5</a>
		<a href="#">选项卡6</a>
	</div>
</div>
<div id="tabs-bd">
	<div class="tabs-bd-scroller">
		<div class="wrapper" >
			<div class="scroller"> 
			<!--这里是内容区域--> <ul><li>...</li></ul>
			</div>
		</div>
		<div class="wrapper" ><div class="scroller"> <ul><li>...</li></ul></div>
	  <div class="wrapper" ><div class="scroller"> <ul><li>...</li></ul></div>
	  <div class="wrapper" ><div class="scroller"> <ul><li>...</li></ul></div>
	  <div class="wrapper" ><div class="scroller"> <ul><li>...</li></ul></div>
	</div>
</div>
```
###JS调用
```javascript
  var ir = new iScrollRefresh('tabs','tabs-bd');
	ir.upAction = pullUp; //上拉刷新的处理函数
	ir.downAction = pullDown; //下拉加载的处理函数
```

### 处理函数说明

- 下拉刷新和上拉加载的回调方法中都会传入一个参数过来
- 使用pullDownCallBack方法来隐藏下拉刷新的提示

```javascript

ir.upAction = pullUp; //定义下拉刷新回调函数
function pullUp(options){
	/*
	控件会把options参数传过来
	options.scroll  当前滚动的ISCROLL对象
	options.index   当前选项卡的索引，从0开始
	options.lastUpdate 上一次刷新的时间
	*/

	var index = options.index,lastUpdate = options.lastUpdate;
	var url = 'www.xxx.com/aticels'
	$.getJSON(url, {lastUpdate: lastUpdate}, function(json, textStatus) {
		if(json.code == 200){
			$('ul li').prepend(parseHtml(json.data));
			//关闭下拉刷新提示
			ir.pullDownCallBack(param); //如果不屏蔽下拉刷新，则第二个参数不要传
		}else{
			ir.pullDownCallBack(param,1); //如果屏蔽下拉刷新，则传第二个参数进去
		}
	});
}

ir.downAction = pullDown; //定义上拉加载的回调函数
function pullDown(options){
	/*
	控件会把options参数传过来
	options.scroll  当前滚动的ISCROLL对象
	options.index   当前选项卡的索引，从0开始
	options.lastUpdate 上一次刷新的时间
	options.page 当前已经加载的页数
	*/

	var index = options.index,lastUpdate = options.lastUpdate,page=options.page;
	var url = 'www.xxx.com/aticels'
	$.getJSON(url, {page: page,lastUpdate:lastUpdate}, function(json, textStatus) {
		if(json.code == 200){
			$('ul li').append(parseHtml(json.data));
			//关闭下拉刷新提示
			ir.pullUpCallBack(param); //如果还有更多数据，则第二个参数不要传
		}else{
			ir.pullUpCallBack(param,1); //如果没有更多数据的时候，传入第二个参数，屏蔽上拉加载
		}
	});
}

```

## 其他方法

```javascript
ir.upAnimation = function(options){...} //上拉刷新的动画设置
ir.downAnimation =function(options){...} //下拉加载的动画设置
/* options说明
scroll:当前iscroll控件对象
index:当前选项卡索引
downTip:上拉刷新的div/下拉加载的div
status:当前状态 0 下拉刷新/下拉加载  1 松开刷新/松开加载 2 刷新中/加载中 3 没有更多数据
*/

ir.refresh(index) //刷新index索引下的iscroll控件
ir.setPage(index,page) //设置index索引下加载的页数
ir.setNoMore(index) //设置index索引没有更多新数据 （下拉刷新没有新数据的情况）
ir.setLoadedAll(index) //设置index索引没有更多数据（上拉加载后全部数据都加载完成的情况）
ir.slide(function(index){...},true) //选项卡切换后回调函数 index为切换后的索引，第二参数如果传了则立即执行第一个function
```













