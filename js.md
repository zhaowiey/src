### 以下是常用的代码收集，没有任何技术含量，只是填坑的积累。转载请注明出处，谢谢。

#### 1. PC - js
- 返回指定范围的随机数(m-n之间)的公式
```javascript
Math.random()*(n-m)+m
```

- [return false](http://stackoverflow.com/questions/1357118/event-preventdefault-vs-return-false)
- [return false](http://www.75team.com/archives/201)
```javascript
// event.preventDefault()会阻挡预设要发生的事件.
// event.stopPropagation()会阻挡发生冒泡事件.
// 而return false则是前面两者的事情他都会做：
// 他会做event.preventDefault();
// 他会做event.stopPropagation();
// 停止callback function的执行并且立即return回来
```

- 防止被Iframe嵌套
```javascript
if(top != self){
    location.href = ”about:blank”;
}
```

- 两种图片lazy加载的方式
第一个By JS中级交流群 成都-猎巫 第二个By 上海-zenki 
```javascript
// @description 准备为图片预加载使用的插件
// 使用的图片容器css类名为lazy-load-wrap
// 图片真实地址为data-lazy-src
// 当lazy-load-wrap容器进入视口，则开始替换容器内所有需要延迟加载的图片路径，并更改容器的加载状态
//第一种方法
$.fn.compassLazyLoad=function(){
	var _HEIGHT=window.innerHeight,
	_lazyLoadWrap=$('.lazy-load-wrap');

	var methods={
		setOffsetTop:function(){
			$.each(_lazyLoadWrap,function(i,n){
				$(n).attr({
					'top':n.offsetTop-_HEIGHT,
					'status':'wait'
				});
			})
		},
		isShow:function(){
			var _scrollTop=$(window).scrollTop;
			//利用image容器判断是否进入视口，而非image本身
			$.each(_lazyLoadWrap,function(){
				var _that=$(this);
				if (_that.attr('status')==='done') {
					return;
				};
				if (_that.attr('top')<=_scrollTop) {
					_that.find('img[data-lazy-src]').each(function(i,n){
						n.src=$(n).data('lazy-src');
					});
					_that.attr('status','done');
				};
			})
		},
		scroll:function(){
			$(window).on('scroll',function(){
				methods.isShow();
			});
		},
		init:function(){
			methods.setOffsetTop();
			methods.isShow();
			methods.scroll();
		}
	};
	methods.init();

}


//第二种方法

var exist=(function($){
	var timer=null,
	temp=[].slice.call($('.container'));
	ret={};

	for(var i=0,len=temp.length-1;i<=len;i++){
		ret[i]=temp[i];
	}
	var isExist=function(winTop,winEnd){
		for(var i in ret){
			console.log(ret);
			var item=ret[i],
			eleTop=item.offsetTop,
			eleEnd=eleTop+item.offsetHeight;

			if((eleTop>winTop&&eleTop<=winEnd)||(eleEnd>winTop&&eleEnd<=winEnd)){
				$(item).css('background','none');
				new Tab($(item).attr('id'),data).init;
				delete ret[i];
			}
		}
	}
	return {
		timer:timer;
		isExist:isExist;
	};
})($);



//第三种方法
Zepto(function ($) {
    var swiper = new Swiper('.swiper-container', {
        pagination: '.swiper-pagination',
        paginationClickable: true,
        autoplay: 3000,
        loop: true,
        autoplayDisableOnInteraction: false
    });
    (function lazyLoad() {
        var imgs = $(".lazyLoad");
        var src = '';
        $.each(imgs, function (index, item) {
            src = $(item).attr('data-src');
            $(item).attr('src', src);
        });
    })();
});
$(function () {
    var lazyLoadTimerId = null;
    /// 智能加载事件
    $(window).bind("scroll", function () {
        clearTimeout(lazyLoadTimerId);
        lazyLoadTimerId = setTimeout(function () {
            // 延迟加载所有图片
            var isHttp = (location.protocol === "http:");
            $("#ym_images img").each(function () {
                var self = $(this);
                if (self.filter(":above-the-fold").length > 0) {
                    var originUrl = self.attr("data-original");
                    self.attr("src", originUrl);
                }
            });
        }, 500);
    });
});

```

- 某年某月的1号为星期几
```javascript
var weekday = ["星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"];
weekday[new Date(2015, 9, 1).getDay()];	//2015年10月1号
```

#### 2. Mobile - js

- [js 判断IOS, 安卓](http://caibaojian.com/browser-ios-or-android.html)
```javascript
var u = navigator.userAgent, app = navigator.appVersion;
var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android终端或者uc浏览器
var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
alert('是否是Android：'+isAndroid);
alert('是否是iOS：'+isiOS);
```

#### 3. [微信 weixin](http://loo2k.com/blog/detecting-wechat-client/)

- UserAgent 判断微信客户端
```javascript
// Mozilla/5.0 (iPhone; CPU iPhone OS 8_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12F70 MicroMessenger/6.1.5 NetType/WIFI
function isWechat() {  
    var ua = navigator.userAgent.toLowerCase();
    return /micromessenger/i.test(ua) || /windows phone/i.test(ua);
}
```

- JS接口安全域名不填写，分享onMenuShareAppMessage直接会取默认值。
```javascript
// 分享onMenuShareAppMessage直接会取默认值
```

- 关闭当前页面
```javascript
WeixinJSBridge.call('closeWindow');
```

- [支付接口方法调用必须在addevent里边调用](http://www.cnblogs.com/true_to_me/p/3565039.html)
```javascript
document.addEventListener('WeixinJSBridgeReady', function onBridgeReady(){
    that.initOrder();
}, false);
```

- 支付接口方法调用必须在
```javascript
WeixinJSBridge.invoke('getBrandWCPayRequest', d, function(res){
    if(res.err_msg == "get_brand_wcpay_request:ok"){
        // alert("支付成功");
        // union.release(d.orderId);
        resetUrl();
        paySuccess('home', d.orderId);
    } else {
        cancelOrder(d.orderId);
        // alert(res.err_msg);
    }
    loading.hide();
});
```

- 瀑布流无限加载实例 
```javascript
// be dependent on jquery & jquery.infinitescroll.min.js
// insert this '<div id="more"><a href="api?page="></a></div>' to your page.html
(function($){
  $(function(){
      var $container = $('.list-wrap-gd');
      function layOutCallBack() {
          $container.imagesLoaded(function(){
              $container.masonry({
                  itemSelector: '.item-bar',
                  gutter: 10
              });
          });
          $container.imagesLoaded().progress( function() {
              $container.masonry('layout');
          });
      }

      layOutCallBack();

      $container.infinitescroll({
          navSelector : "#more",
          nextSelector : "#more a",
          itemSelector : ".item-bar",
          pixelsFromNavToBottom: 300,
          loading:{
              img: "/images/masonry_loading.gif",
              msgText: ' ',
              finishedMsg: "<em>已经到最后一页</em>",
              finished: function(){
                  $("#more").remove();
                  $("#infscr-loading").hide();
              }
          },
          errorCallback:function(){
              $(window).unbind('.infscr');
          },
          pathParse: function (path, nextPage) {
              var query = "";
              var keyword=$("#search_keyword").val();
              var cat_id=$("#cat_id").val();
              var brand_id=$("#brand_id").val();
              var country_id = $("#country_id").val();
              query = query + "&namekeyword="+keyword;
              query = query +"&cat_id="+cat_id
              query = query + "&brand_id=" + brand_id; 
              query = query + "&country_id=" + country_id;
              path = [path,query];
              return path;
          }
      },

      function(newElements) {
          var $newElems = $( newElements ).css({ opacity: 0 });
          $newElems.imagesLoaded(function(){
              $newElems.animate({ opacity: 1 });
              $container.masonry( 'appended', $newElems, true );
              layOutCallBack();
          });
      });
  });
})(jQuery);
```
  
- [iOS，Safari浏览器，input等表单focus后fixed元素错位问题](https://www.snip2code.com/Snippet/176582/--iOS-Safari----input---focus-fixed-----)
```javascript
if( /iPhone|iPod|iPad/i.test(navigator.userAgent) ) {
  $(document).on('focus', 'input, textarea', function()
  {
     $('header').css("position", 'absolute');
     $('footer').css("position", 'absolute');
     
  });
  
  $(document).on('blur', 'input, textarea', function()
  {
       $('header').css("position", 'fixed');
       $('footer').css("position", 'fixed');
      
  });
} 

```
  
- 得到地理位置
```javascript
function getLocation(callback){
  if(navigator.geolocation){
      navigator.geolocation.getCurrentPosition(
              function(p){
                  callback(p.coords.latitude, p.coords.longitude);
              },
              function(e){
                  var msg = e.code + "\n" + e.message;
              }
      );
  }
}
```

- [rem计算适配](http://isux.tencent.com/web-app-rem.html)
```javascript
(function(doc, win){
  var docEl = doc.documentElement,
      resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
      recalc = function(){
          var clientWidth = docEl.clientWidth;
          if(!clientWidth) return;
          docEl.style.fontSize = 20 * (clientWidth / 320) + 'px';
      };

  if(!doc.addEventListener) return;
  win.addEventListener(resizeEvt, recalc, false);
  doc.addEventListener('DOMContentLoaded', recalc, false);
})(document, window);
```

- [另外一种rem方案](http://www.html-js.com/article/3041)
```javascript
var dpr, rem, scale;
var docEl = document.documentElement;
var fontEl = document.createElement('style');
var metaEl = document.querySelector('meta[name="viewport"]');

dpr = window.devicePixelRatio || 1;
rem = docEl.clientWidth * 2 / 10;
scale = 1 / dpr;


// 设置viewport，进行缩放，达到高清效果
metaEl.setAttribute('content', 'width=' + dpr * docEl.clientWidth + ',initial-scale=' + scale + ',maximum-scale=' + scale + ', minimum-scale=' + scale + ',user-scalable=no');

// 设置data-dpr属性，留作的css hack之用
docEl.setAttribute('data-dpr', dpr);

// 动态写入样式
docEl.firstElementChild.appendChild(fontEl);
fontEl.innerHTML = 'html{font-size:' + rem + 'px!important;}';

// 给js调用的，某一dpr下rem和px之间的转换函数
window.rem2px = function(v) {
    v = parseFloat(v);
    return v * rem;
};
window.px2rem = function(v) {
    v = parseFloat(v);
    return v / rem;
};

window.dpr = dpr;
window.rem = rem;
```