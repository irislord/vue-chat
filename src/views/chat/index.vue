<template>
  <transition>
    <!-- 绑定style中的top和left来实现拖拽 -->
    <div class="chatBox" v-show="isShow" :style="position">
      <!-- 禁用h5原生的拖拽事件，否则会与mouseup事件冲突，双击后导致拖拽失效 -->
      <section class="left-section"></section>
      <section class="right-section">
        <div
          class="top-bar"
          @mousedown="mousedown"
          @mouseup="mouseup"
          @dragstart="(e) => e.preventDefault()"
          @dragend="(e) => e.preventDefault()"
        ></div>
        <div class="chat-container">
          <chat-list></chat-list>
          <chat-content></chat-content>
        </div>
      </section>
    </div>
  </transition>
</template>

<script>
import chatList from '@/views/chat/components/ChatList.vue'
import chatContent from '@/views/chat/components/ChatContent.vue'
export default {
  components: {
    chatList: chatList,
    chatContent: chatContent
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
      const _this = this
      this.leftOffset = event.offsetX
      this.topOffset = event.offsetY
      this.isMove = true
      document.onmousemove = function (event) {
        if (!_this.isMove) {
          return
        }
        _this.x = event.clientX - _this.leftOffset
        _this.y = event.clientY - _this.topOffset
      }
    },
    mouseup () {
      this.isMove = false
      document.onmousemove = null
      console.log(this.isMove)
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
    position: fixed;
    .left-section{
        width: 10%;
        height: 100%;
        background-color: #777;
        float: left;
    }
    .right-section{
        width: 100%;
        height: 100%;
        background-color: #ff7;
        .top-bar{
          width: 100%;
          height: 10%;
          background-color: #f77;
        }
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