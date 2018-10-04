<template>
  <div id="wrapper" ref="wrapper" :class="className">
    <div class="scroller" :class="{'scroll-static': showIcon && !scrolling, 'no-scroll': !up && !down}" ref="scroller">
      <div class="scroll-head-mask" v-if="up" :style="headbg"></div>
      <div v-if="up" :style="headbg" class="scroll-head tc">
        <span :style="iconColor" class="pull-icon mr8" :class="{'icon-static': showIcon}"></span>
        <span :style="textColor" class="head-text">{{ headText }}</span>
      </div>
      <slot>
        <!-- <div class="tc">暂无数据</div> -->
      </slot>
      <div class="scroll-foot" v-if="down">
        <div v-show="!allLoaded">{{ footText }}</div>
        <div v-show="allLoaded">全部加载完成</div>
      </div>
    </div>
  </div>
</template>
<script>
import abc from '@/assets/plugins/abc'
import '@/assets/plugins/iscroll-probe'
/**
 * @author
 * @description 下拉刷新，上拉加载组件
 * @date 2018-7-4
 * @param value Boolean 控制关闭刷新,IScroll重新计算滚动高度
 * @param up Boolean 是否需要下拉刷新
 * @param down Boolean 是否需要上拉刷新
 * @param allLoaded Boolean 是否全部加载完毕
 * @param refreshColor Object 自定义头部样式
 * @param className String wrapper类名
 * @param getScrilling Boolean 滚动时是否需要scroll实例信息
 * @event refresh 下拉刷新事件
 * @event getMore 上拉加载更多
 * @method scrollIntoView 使内容滚动到视图区域，同原生srollIntoView,默认参数true
 */
export default {
  name: 'VScroll',
  props: {
    value: {
      type: [Boolean, Number],
      default: true
    },
    up: {
      type: [Boolean, Number],
      default: false
    },
    down: {
      type: [Boolean, Number],
      default: false
    },
    allLoaded: {
      type: [Boolean, Number],
      default: false
    },
    refreshColor: {
      type: Object,
      default() {
        return {
          loadBgColor: '#ffffff', // 背景色
          textColor: '#999', // 文本颜色
          cirCleColor: '#256ef6' // 圈颜色
        }
      }
    },
    className: String,
    getScrilling: Boolean
  },
  data() {
    return {
      myScroll: null, // IScroll实例
      observer: null, // 观察者实例
      headText: '下拉刷新', // 头部提示
      footText: '上拉加载更多', // 底部提示
      showIcon: false, // 显示请求状态
      timer: 0, // 刷新时请求的计时器
      minTimer: 1000, // 刷新最少延时
      maxTimer: 29000, // 刷新最大延时(实际是30秒)
      scrolling: false, // 滚动时
      recordOldHeight: '0' // // 记录下旧的高度，避免重复触发回调函数
    }
  },
  computed: {
    // 计算头部样式
    headbg() {
      const r = this.refreshColor && this.refreshColor.loadBgColor
      return r
        ? `background-color:${this.refreshColor.loadBgColor}`
        : 'background-color: white'
    },
    // 头部文字样式
    textColor() {
      var textColor = '#999'
      if (this.refreshColor && this.refreshColor.textColor) {
        if (this.refreshColor.textColor) {
          textColor = this.refreshColor.textColor
        }
      }
      textColor = 'color:' + textColor + ';' + this.headbg
      return textColor
    },
    // icon样式
    iconColor() {
        let iconColor = ''
      this.refreshColor &&
        this.refreshColor.cirCleColor &&
        (iconColor = this.refreshColor.cirCleColor)
      return `color: ${iconColor || '#256ef6'}`
    }
  },
  watch: {
    value(a) {
      if (!a) {
        const _this = this
        let timer = this.minTimer - (+new Date() - +new Date(this.timer))
        timer = timer > 0 ? timer : 800
        setTimeout(() => {
          _this.showIcon = a
          _this.headText = '下拉刷新'
          _this.footText = '上拉加载更多'
          _this.$store.commit('SET_LOADING_STATE', '1')
        }, timer)
      }
    }
  },
  mounted() {
    console.log(abc, IScroll)
    this.initLoad()
    this.mutaionObserver()
  },
  beforeDestroy() {
    this.myScroll && this.myScroll.destroy && this.myScroll.destroy()
    this.myScroll = null
    if (this.observer) {
      this.observer.disconnect()
      this.observer.takeRecords()
      this.observer = null
    }
  },
  methods: {
    mutaionObserver() {
      const MutationObserver = window.MutationObserver || window.WebKitMutationObserver || window.MozMutationObserver
      const scroller = this.$refs.scroller
      this.observer = new MutationObserver(() => {
        // console.log(mutationList, height)
        const height = getComputedStyle(scroller).getPropertyValue('height')
        if (height !== this.recordOldHeight) {
          this.recordOldHeight = height
          this.myScroll.refresh()
        }
      })
      this.observer.observe(scroller, {
        childList: true,
        subtree: true,
        attributeFilter: ['client-height']
      })
    },
    initLoad() {
      let that = this
      const options = {
        mouseWheel: true, // 是否支持鼠标滚轮
        scrollX: false, // 是否显示横向滚动条
        bounce: this.up || this.down, // 是否弹跳效果
        probeType: 2, // 为2的时候，会在屏幕滑动的过程中实时的派发scroll，为3，实时的派发scroll事件,可能会卡顿
        preventDefault: false // 不阻止默认事件
        // click: /iphone|mac/ig.test(window.navigator.platform), // 解决ios上滑动后还是响应了点击事件的问题 在bt-active中处理
        // preventDefaultException: {
        //   tagName: /^(SPAN|I|P|B|UL|LI|H1|H2|DIV|A|INPUT|TEXTAREA|BUTTON|SELECT|IMG)$/
        // }
      }
      // 初始化IScroll
      this.myScroll = new IScroll(this.$refs.wrapper, options)
      const wrapper = this.$refs.wrapper
      const scroller = this.$refs.scroller
      this.myScroll.on('beforeScrollStart', () => {
        this.myScroll.y === 0 &&
          this.headText !== '下拉刷新' &&
          this.myScroll._translate(0, 40)
      })
      // 开始滚动
      this.myScroll.on('scrollStart', () => {
        if (!this.myScroll) return
        this.scrolling = true
        this.myScroll.refresh()
      })
      // 滚动时
      this.myScroll.on('scroll', () => {
        if (!this.myScroll) return
        this.getScrilling && this.$emit('getScrollData', this.myScroll)
        const y = this.myScroll.y
        if (that.up && y > 0) {
          that.headText !== '正在加载...' &&
            y < 40 &&
            (that.headText = '下拉刷新')
          that.headText !== '正在加载...' &&
            y >= 40 &&
            (that.headText = '释放刷新')
        }
        const fh = scroller.clientHeight - wrapper.clientHeight + y
        if (
          that.down &&
          y < 0 &&
          fh < 0 &&
          that.footText === '上拉加载更多' &&
          !that.allLoaded
        ) {
          that.footText = '正在加载...'
          that.$emit('input', true)
          that.$store.commit('SET_LOADING_STATE', '0')
          setTimeout(() => {
            that.$emit('getMore')
          }, 1000)
          that.maxDelayed()
        }
      })
      // 滚动结束
      this.myScroll.on('scrollEnd', () => {
        if (!this.myScroll) return
        this.getScrilling && this.$emit('getScrollData', this.myScroll)
        this.myScroll.y === 0 &&
          this.headText !== '下拉刷新' &&
          (this.scrolling = false)
      })
      // 触摸结束
      this.$refs.wrapper.addEventListener('touchend', () => {
        if (!this.myScroll) return
        this.myScroll.y >= 0 &&
          this.headText !== '下拉刷新' &&
          (this.scrolling = false)
        if (this.headText === '释放刷新' && !this.showIcon && this.up) {
          this.showIcon = true
          this.timer = new Date()
          this.headText = '正在加载...'
          this.$store.commit('SET_LOADING_STATE', '0')
          this.$emit('input', true)
          setTimeout(() => {
            this.$emit('refresh')
            this.maxDelayed()
          }, 20)
        }
      })
    },
    // 请求最大延时
    maxDelayed() {
      setTimeout(() => {
        this.$emit('input', false)
        this.$store.commit('SET_LOADING_STATE', '1')
      }, this.maxTimer)
    },
    // 滚动内容到视图区域，外层调用，勿删
    scrollIntoView(p = true) {
      if (p) {
        this.myScroll.scrollTo(0, 0)
      } else {
        this.$nextTick(() => {
          this.myScroll.refresh()
          this.myScroll.scrollTo(0, this.myScroll.maxScrollY, 200)
        })
      }
    },
    // 刷新icroll
    refresh() {
      this.$nextTick(() => {
        this.myScroll.refresh()
      })
    }
  }
}
</script>
<style lang="scss" scoped>
#wrapper {
  height: 100%;
  overflow: hidden;
  .scroller {
    width: 100%;
    min-height: calc(100% + 1.5px);
    position: relative;
    display: flex;
    flex-flow: column;
    &.no-scroll {
      min-height: 100%;
    }
    &.scroll-static {
      transform: translate(0px, 40px) translateZ(0px) !important;
      transition-duration: 400ms !important;
    }
  }
  .scroll-head {
    height: 2.5rem;
    position: absolute;
    top: -2.5rem;
    left: 0;
    right: 0;
    height: 2.5rem;
    line-height: 2.5rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .scroll-head-mask {
    height: 50vh;
    position: absolute;
    top: -50vh;
    left: 0;
    right: 0;
  }
  .scroll-foot {
    > div {
      height: 35px;
      text-align: center;
      line-height: 35px;
      color: #bdbdbd;
    }
  }
}
.pull-icon {
  display: inline-block;
  box-sizing: border-box;
  width: 32px;
  height: 32px;
  border: 4px solid;
  border-left: 4px solid transparent;
  border-top: 4px solid transparent;
  border-radius: 50%;
  transform: rotate(45deg);
  animation: none;
}
.icon-static {
  animation: rotate-360 1s linear infinite;
}
@keyframes rotate-360 {
  from {
    transform: rotate(45deg);
  }
  to {
    transform: rotate(405deg);
  }
}
</style>
