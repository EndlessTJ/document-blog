> 最近在项目开发中，因为有视频插入的需求。所以使用了`video`标签。但是在测试的过程中，尤其是移动端的测试中遇到了很多问题。上网搜索发现`video`在开发过程中有许多的坑。所以这里把我遇到的情况和网上收集到的问题记录下来。

> 首先，主要问题分为以下几大类：    
1. 全屏加载问题 
2. 安卓手机自动播放问题
3. 播放控制
4. 播放控件的显示与隐藏
5. 腾讯x5内核浏览器和国产安卓内置浏览器接管video将其置于最上层的问题



#### 首先罗列一下video中的一些属性

        src="http://****.mp4"  //src是视频源的地址，当然也可以使用<source>标签来加载视频源
        controls = "true" //是否显示控制台，允许许用户控制视频的播放，包括音量，跨帧，暂停/恢复播放。
        poster = "http://***.png"//视频还未下载或播放时，显示的图像。如果没有设置该属性，pc上默认显示第一帧图像。移动端因设备而异。
        preload = "auto"// 该属性规定，在页面加载完成后载入视频
        webkit-playsinline/playssinline："true"//视频播放时局域播放，不脱离文档流。但这个属性比较特别，需要嵌入网页app，比如WeChat的UIwebview的allowsInlineMediaPlayback=YES才能生效
        //因为安装手机的app不支持playsinline属性，所以安卓手机WeChat播放视频重视全屏，二ISO是支持的。
        x-webkit-airplay="allow"// 该属性使该视频支持iso的AirPlay功能。使用AirPlay可以直接从使用iOS的设备上的不同位置播放视频、音乐还有照片文件，也就是说通过AirPlay功能可以实现影音文件的无线播放，当然前提是播放的终端设备也要支持相应的功能。
        x5-video-player-type="h5"// 启用h5同城播放器，就是在视频全屏的时候，div可以呈现在视频层上，也是WeChat安卓版特有的属性。
        //同层播放别名也叫做沉浸式播放，播放的时候看似全屏，但是已经除去了control和微信的导航栏，只留下"X"和"<"两键。目前的同层播放器只在Android（包括微信）上生效，暂时不支持iOS。至于为什么同层播放只对安卓开放，是因为安卓不能像ISO一样局域播放，默认的全屏会使得一些界面操作被阻拦，如果是全屏H5还好，但是做直播的话，诸如弹幕那样的功能就无法实现了，所以这时候同层播放的概念就解决了这个问题。不过在测试的过程中发现，不同版本的IOS和安卓效果略有不同
        
        x5-video-orientation="portraint"//播放器支付的方向， landscape横屏，portraint竖屏，默认值为竖屏
          x5-video-player-fullscreen="true" /*全屏设置，
                     设置为 true 是防止横屏*/
        
        
        
#### 全屏处理
- **iso**
- - ios加playsinline属性，之前只带webkit前缀的在ios10以后，会吊起系统自带播放器，两个属性都加上基本ios端都可以保证内敛到浏览器webview里面了。如果仍有个别版本的ios会吊起播放器，还可以引用一个库iphone-inline-video（具体用法很简单看它github，这里不介绍了，只需加js一句话，css加点），github地址加上playsinline webkit-playsinline这两个属性和这个库基本可以保证ios端没有问题了（不过亲测，只加这两个属性不引入库好像也是ok的，至今没有在ios端微信没有出现问题，如果你要兼容uc或者qq的浏览器建议带上这个库）.
- **android**
- - x5-video-player-type="h5"属性，腾讯x5内核系的android微信和手Q内置浏览器用的浏览器webview的内核，使用这个属性在微信中视频会有不同的表现，会呈现全屏状态，貌似播放控件剥去了，至少加了这个属性后视频上层可以有其他dom元素出现了（非腾讯白名单机制的一种处理措施）

#### 自动播放
- android始终不能自动播放，不多说。值得一提的是经测现在ios10后版本的safari和微信都不让视频自动播放了（顺带音频也不能自动播放了），但微信提供了一个事件WeixinJSBridgeReady，在微信嵌入webview全局的这个事件触发后，视频仍可以自动播放，这个应该是现在在ios端微信的视频自动播放的比较靠谱的方式，其他如手q或者其他浏览器，建议就引导用户触发触屏的行为操作出发比较好。

#### 播放控制
- 对于video或者audio等媒体元素，有一些方法，常用的有play(),pause();也有一些事件，如loadstart,canplay,canplaythrough,ended,timeupdate....等等。
在移动端有一些坑需要注意，不要轻易使用媒体元素的除ended,timeupdate以外event事件，在不同的机子上可能有不同的情况产生，例如：ios下监听canplay和canplaythrough（是否已缓冲了足够的数据可以流畅播放）,当加载时是不会触发的，即使preload="auto"也没用，但在pc的chrome调试器下和android下，是会在加载阶段就触发。ios需要播放后才会触发。总之就是现在的视频标准还不尽完善，有很多坑要注意，要使用前最好自己亲测一遍。就是当第一次播放视频的时候ios端，如果网络慢，视频从开始播到能展现画面会有短暂的黑屏（处理视频源数据的时间），为了避免这个黑屏，可以在视频上加个div浮层（可以一个假的视频第一帧），然后用timeupdate方法监听，视屏播放及有画面的时候再移除浮层。

        video.addEventListener('timeupdate',function (){
    //当视频的currentTime大于0.1时表示黑屏时间已过，已有视频画面，可以移除浮层（.pagestart的div元素）
    if ( !video.isPlayed && this.currentTime>0.1 ){
        $('.pagestart').fadeOut(500);
        video.isPlayed = !0;
    }
})

#### 播放控件的隐藏
- x5-video-player-type="h5"属性会隐藏播放控件，上层可以浮div或者其他元素了，还有一点值得说的是，带播放器控件的隐藏.

        <div class="videobox" ontouchmove="return false;">
            <video id="mainvideo" src="test.mp4" x5-video-player-type="h5" playsinline webkit-playsinline></video>
        </div>
 比如这个videobox在android下隐藏，只用display：none貌似还是不行的，但加个z-index:-1，设置成-1就可以达到隐藏播放器控件的目的了。
 
 #### 腾讯x5内核浏览器和国产安卓内置浏览器接管video将其置于最上层的问题
 
 - x5浏览器添加x5-video-player-type="h5"，部分国产安卓内置浏览器暂时还没办法处理，只能保证不要在视频上方添加dom元素了。