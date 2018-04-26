<template>
  <div class="f-carousel-wrapper" @mouseover="stopAutoPlay" @mouseout="startAutoPlay">
    <div class="f-carousel-content">
      <transition-group tag="ul" class='f-content-ul' :name="direction">
        <li v-for="(list,index) in slideList" :key="index" v-show="index===currentIndex">
          <img :src="list.image">
        </li>
      </transition-group>
      <h4 class="f-content-title">{{slideString}}（{{currentIndex+1}}/{{slideList.length}}）</h4>
    </div>
    <div class="f-carousel-trigger">
      <button class="f-trigger-left" @click="forward" v-show="slideOption.isLoop||currentIndex!==0"> < </button>
      <button class="f-trigger-right" @click="backward" v-show="slideOption.isLoop||currentIndex!==slideList.length-1"> > </button>
    </div>
  </div>
</template>
<script>
function throttle(obj, fn, interval = 100) {
  let _self = fn
  return (function () {
    let args = arguments
    let _me = this
    if (obj.timeFlag) {
      _self.apply(_me, args)
      obj.timeFlag = false
      obj.throttleTimer = setTimeout(function () {
        clearTimeout(obj.throttleTimer)
        obj.throttleTimer = null
        obj.timeFlag = true
      }, interval)
    }
  })()
}
export default {
  props: {
    list: {
      type: Array,
      default: []
    },
    title: {
      type: String,
      default: ''
    },
    option: {
      type: Object,
      default() {
        return {
          isLoop: true,
          isAutoLoop: true,
          loopTime: 3000
        }
      }
    }
  },
  data() {
    return {
      currentIndex: 0,
      direction: 'forward',
      slideList: [],
      slideString: '',
      slideOption: {},
      searchThrottleConfig: {
        timeFlag: true,
        throttleTimer: null
      },
      timer: null
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      this.slideList = this.list
      this.slideString = this.title
      this.slideOption = this.option
      this.initOption()
    },
    initOption() {
      if (this.slideOption.isAutoLoop) {
        this.autoPlay()
      }
    },
    autoPlay() {
      let that = this
      this.timer = setInterval(function() {
        that.backward()
      }, this.slideOption.loopTime)
    },
    stopAutoPlay() {
      clearInterval(this.timer)
      this.timer = null
    },
    startAutoPlay() {
      if (this.slideOption.isAutoLoop) {
        this.autoPlay()
      }
    },
    forward() {
      let that = this
      return throttle(this.searchThrottleConfig, function () {
        that.direction = 'forward'
        if (that.currentIndex <= 0) {
          that.currentIndex = that.slideList.length - 1
        } else {
          that.currentIndex--
        }
      }, 500)
    },
    backward() {
      let that = this
      return throttle(this.searchThrottleConfig, function () {
        that.direction = 'backward'
        if (that.currentIndex >= that.slideList.length - 1) {
          that.currentIndex = 0
        } else {
          that.currentIndex++
        }
      }, 500)
    }
  }
}
</script>
<style lang="stylus" scoped>
.f-carousel-wrapper
  position relative
  .f-carousel-content
    height 450px
    width 50%
    margin 0 auto
    overflow hidden
    background-color #fff
    .f-content-ul
      position relative
      width 100%
      height 90%
      li
        position absolute
        width 100%
        height 100%
      img
        width 100%
        height 100%
    .f-content-title
      margin-top 10px
  .f-carousel-trigger
    .f-trigger-left, .f-trigger-right
      position absolute
      transition background .3s
      height 80px
      width 80px
      line-height 80px
      cursor pointer
      border-radius 50%
      top 50%
      color #fff
      transform translateY(-50%)
      background rgba(31,45,61,.11)
      border none
      outline none
      padding 0
      margin 0
      font-size 40px
    .f-trigger-left:hover, .f-trigger-right:hover
      background rgba(31,45,61,.22)
    .f-trigger-left
      left 224px // get my love girl
    .f-trigger-right
      right 224px
// 过渡操作
.forward-enter-to, .backward-enter-to
  transition all 1s ease
  transform translateX(0)
.forward-leave-active
  transition all 1s ease
  transform translateX(100%)
.forward-enter
  transform translateX(-100%)
.backward-leave-active
  transition all 1s ease
  transform translateX(-100%)
.backward-enter
  transform translateX(100%)
.forward-leave, .backward-leave
  transform translateX(0)
</style>
