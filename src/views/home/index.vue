<template>
  <div>
    <button @click="$refs.chat.showChatBox()">chat</button>
    <chat ref="chat"></chat>
  </div>
</template>

<script>
import chat from '@/views/chat/index'
export default {
  components: {
    chat: chat
  },

  name: 'VueChatIndex',

  data () {
    return {}
  },

  mounted () {},

  methods: {}
}
</script>

<style lang="scss" scoped>

</style>