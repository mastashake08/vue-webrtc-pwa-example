<template>
  <div id="app">
  <input v-model="roomId" placeholder="Room ID"/>
  <vue-webrtc width="100%" :roomId="roomId" stunServer="stun.l.google.com:19302" ref="webrtc"
  v-on:joined-room="logEvent"
                      v-on:left-room="logEvent"
                      v-on:opened-room="logEvent"
                      v-on:share-started="logEvent"
                      v-on:share-stopped="logEvent"
                      @error="onError"></vue-webrtc>
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
      this.$refs.webrtc.shareScreen()
    },
    takePhoto: function() {
      console.log("Take Photo")
      this.$refs.webrtc.capture()
    },
    joinRoom: function(){
      console.log('Join Room')
      this.$refs.webrtc.join()

    },
    leaveRoom: function() {
      this.$refs.webrtc.leave()
    },
    onError(error, stream) {
        console.log('On Error Event', error, stream);
      },
    logEvent(event) {
      console.log('Event : ', event);
    },
  },
  mounted(){
    this.joinRoom()
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
