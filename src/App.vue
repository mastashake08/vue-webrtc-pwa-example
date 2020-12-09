<template>
  <div id="app">
  <input v-model="roomId" placeholder="Room ID"/>
  <vue-webrtc width="100%" :roomId="roomId" ref="rtc"></vue-webrtc>
  <button v-on:click="joinRoom()">Join Room 🤝</button>
  <button v-on:click="leaveRoom()">Leave Room ❌</button>
  <button v-on:click="screenShare()">Share Screen 🖥️</button>
  <button v-on:click="takePhoto()">Take Photo 📷</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      roomId: 'test'
    }
  },
  methods: {
    screenShare: function() {
      console.log("Screen Share")
      this.$refs.rtc.shareScreen()
    },
    takePhoto: function() {
      console.log("Take Photo")
      this.$refs.rtc.capture()
    },
    joinRoom: function(){
      this.$refs.rtc.join()
    },
    leaveRoom: function() {
      this.$refs.rtc.leave()
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
