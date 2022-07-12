<template>
  <transition>
    <!-- 绑定style中的top和left来实现拖拽 -->
    <div class="chatBox" v-show="isShow" :style="position">
      <!-- 禁用h5原生的拖拽事件，否则会与mouseup事件冲突，双击后导致拖拽失效 -->
      <section
        class="top-section"
        @mousedown="mousedown"
        @mouseup="mouseup"
        @mousemove="mousemove"
        @dragstart="(e) => e.preventDefault()"
        @dragend="(e) => e.preventDefault()"
      ></section>
      <section class="left-section"></section>
      <section class="right-section"></section>
    </div>
  </transition>
</template>

<script>
import chatBox from '@/views/chat/chatBox.vue'
export default {
  components: {
    chatBox: chatBox
  },
  name: 'VueChatIndex',
  props: {},

  data () {
    return {
      isShow: false,
      isMove: false,
      x: window.innerWidth / 2 - 300,
      y: window.innerHeight / 2 - 250,
      leftOffset: 0,
      topOffset: 0
    }
  },
  computed: {
    position () {
      return `left:${this.x}px;top:${this.y}px`
    }
  },

  mounted () {},

  methods: {
    showChatBox () {
      this.isShow = !this.isShow
    },
    mousedown (event) {
      this.leftOffset = event.offsetX
      this.topOffset = event.offsetY
      this.isMove = true
    },
    mouseup () {
      this.isMove = false
    },
    mousemove (event) {
      if (!this.isMove) {
        return
      }
      this.x = event.clientX - this.leftOffset
      this.y = event.clientY - this.topOffset
    }
  }
}
</script>

<style lang="scss" scoped>
.chatBox{
    width: 600px;
    height: 500px;
    position: absolute;
    .top-section{
        width: 100%;
        height: 10%;
        background-color: #f77;
    }
    .left-section{
        width: 10%;
        height: 90%;
        background-color: #777;
        float: left;
    }
    .right-section{
        width: 90%;
        height: 90%;
        background-color: #ff7;
        float: right;
    }
}
.v-enter,.v-leave-to{
    opacity: 0;
    transform: scale(0.5);
}
.v-enter-active,
.v-leave-active {
  transition: all 0.5s ease;
}
</style>