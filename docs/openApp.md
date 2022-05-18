### 情景
    H5页面在微信网页中或者浏览器网页中尝试调起手机APP（微信网页限制调起App）

### 实现
```html
<template v-if="browser.versions.weixin && token"> <!-- 在微信浏览器中并且已登录，使用微信打开app的组件 -->
  <wx-open-launch-app appid="wx953f03f31f089a16" :extinfo='`hao5dao://?order_sn=${order_sn}&type=11`' @error="(e) => handleError(e.detail)" @ready="handleReady" @launch="handleLaunch">
    <script type="text/wxtag-template">
      <style>.wximg { width: 333px }</style>
      <div>
        <img src="https://kgtimage.kgtmall.com.cn/upload/20211204/11173110433.gif" alt="" class="wximg">
      </div>
    </script>
  </wx-open-launch-app>
</template>
<template v-else> <!-- 在浏览器中展示普通标签 -->
  <div @click="joinClick">
    <img src="https://kgtimage.kgtmall.com.cn/upload/20211204/11173110433.gif" alt="">
  </div>
</template>

<!-- 跳转浏览器提示 -->
<van-overlay :show="showPic" @click="showPic = false" z-index="999">
  <div class="wrapper" @click.stop>
    <img src="@/assets/img/liulanqi.png" alt="" width="300px">
  </div>
</van-overlay>
```
```js
methods: {
  joinClick() { // 在浏览器中打开app，如果手机未安装app，等待几秒钟然后跳转去appstore下载
    if (getToken()) {
      if (browser.versions.ios) {
        window.location.href = `hao5dao://?order_sn=${this.order_sn}&type=11`
        setTimeout(() => {
          window.location.href = "https://apps.apple.com/cn/app/id1523978737"
          window.location.href = "https://apps.apple.com/cn/app/id1523978737"
        },2000)
      } else if (browser.versions.android) {
        window.location.href = `hao5dao://?order_sn=${this.order_sn}&type=11`
        setTimeout(() => {
          window.location.href = "https://sj.qq.com/myapp/detail.htm?apkName=com.tencent.tmgp.sgame&info=607E2D024EA7186CD5AF14AA032013A0"
        },2000)
      }
    } else {
      // 未登录跳转去登录 （登录流程可省略）
      this.$router.push({
        path: '/login',
        query: {
          referrer: this.referrer
        }
      })
    }
  }
  handleError(err) {
    if (err.errMsg == 'launch:fail') { // 因为打开app失败，就跳转到应用下载软件
     if (browser.versions.ios) {
      window.location.href = "https://apps.apple.com/cn/app/id1523978737";
     } else if (browser.versions.android) {
      window.location.href = "https://sj.qq.com/myapp/detail.htm?apkName=com.tencent.tmgp.sgame&info=607E2D024EA7186CD5AF14AA032013A0"
     }
    // this.$router.push({
    //   path: '/login',
    //   query: {
    //     referrer: this.referrer
    //   }
    // })
    } else { // 其他情况展示在浏览器中打开提示
      this.showPic = true
    }
  }
  handleReady() {
    console.log('组建加载完毕')
  }
  handleLaunch() {
    console.log('launch')
  }
}
```
> 发现问题：复制的链接在微信中打开是不能打开App的，只有分享的公众号卡片或者其他链接可以
