<template>
  <div id="mar">
    <!--主div分三部分，图标，系统公告名字，展示区域-->
    <div class="header">
      <!--图标-->
      <!--名字-->
      <span class="announcement">系统公告：</span> </div>
    <!--公告展示块-->
    <div class="PackagingShell" id="PackagingShell">
      <!--主题内容，展示内容区域-->
      <div id="viewBox">
        <!--文本主体-->
        <span id="marquee">{{text}}</span>
        <!--文本副本，实现无缝滚动-->
        <span id="transcript" ></span>
      </div>
      <!--隐藏块，用于获取展示文本宽度-->
      <div id="hide">{{text}}</div>
    </div>
  </div>
</template>

<script>
// import {getHomepageMessage} from '@/api/getdata'
export default {
  name: 'Marquee',
  data () {
    return {
      text: '', // 内容
      textWidth: 0, // 字符长度
      isScroll: false // 是否滚动
    }
  },
  methods: {
    init () { // 初始化函数
    	this.text='滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试'
      getHomepageMessage().then((response) => { // 接口请求
        if (response.body.code !== '0000') {
          return
        }
        // this.text = response.body.data[0].messageContent
        // 滚动内容赋值
        this.text = '滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试滚动无缝链接测试'
      })
        .catch((response) => {
          console.log(response, 'error')
        })
    },
    move () {
      // 首先获取整个内容的宽度,(展示内容会超出隐藏,无法通过获取盒子宽度取到实际的内容宽度)
      // 所以专门造了一个 div 块来放内容,隐藏起来 (无法用根据字体内容来计算宽度的方法,存在误差,当内容较多
      // 的时候误差会相对较大)
      let width = document.getElementById('hide').getBoundingClientRect().width
      // 获取展示块盒模型宽度
      let BoxWidth = document.getElementById('PackagingShell').offsetWidth
      // 获取内容展示宽
      let viewBox = document.getElementById('viewBox')
      // 判断内容是否超长
      if ((BoxWidth - width) < 0) {
        // 内容超长,则获取抄本元素
        let transcript = document.getElementById('transcript')
        // 抄本元素内容填充
        transcript.innerText = this.text
        // 设定抄本和主题之间的间距
        transcript.style.marginLeft = '200px'
      } else {
        // 没有超长则return,不做任何操作
        return
      }
      // 设置初始位移距离
      let distance = 0
      // 移动函数,通过定时器实现
      setInterval(function () {
        // 位移内用记录是是递减,此处的 1 控制移动变量 s
        distance = distance - 1
        // 判断是否整个内容移动完
        if (-distance >= width) {
          // 若移动完,则重新设定位移值,此处赋值200是正好把 上面设置的块间距  200px 也加入移动范围,实现移动完无缝跳转
          distance = 200
        }
        // 实时设置位移距离
        viewBox.style.transform = 'translateX(' + distance + 'px)'
      }, 27) // 移动时间间隔t   s和t 共同决定移动速度
    }
  },
  mounted () {
    // 初始化
    this.init()
  },
  // 更新的时候运动
  updated: function () {
    this.move()
  }
}
</script>

<style scoped lang="scss">
  #mar {
    /*系统公告字体颜色*/
    .announcement{
      color:#ff4786;
    }
    height:40px;
    line-height: 40px;
    /*公告滚动盒子样式*/
    .PackagingShell {
      width: 89%;        /*滚动部分宽度*/
      overflow: hidden;  /*超出部分隐藏*/
      font-size: 12px;
      color: #111;
    }
    /*主题内容块设置*/
    #viewBox {
      /*设置内容框不可换行,确保两个div块始终在同一行*/
      white-space: nowrap;
    }
    
    #marquee {
      /*设置内容块不可换行*/
      white-space: nowrap;
    }
    /*获取宽度的块，用z-index隐藏*/
    #hide {
      position: absolute;
      z-index: -999;
      top: -9999px;
      white-space: nowrap;
    }
    /*浮动设置和图片设置*/
    .header {
      float: left;
      font-size:12px;
      >img{
        vertical-align: middle;
      }
    }
  }
</style>