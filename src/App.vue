<template>
  <div id="app">
  <input v-model="roomId" placeholder="Room ID"/>
  <video id="localVideo" autoplay playsinline></video>
  <video id="remoteVideo" autoplay playsinline></video>
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
      roomId: 'test',
      localStream: null,
      localPeerConnection: null
    }
  },
  methods: {
    createdOffer: function(description) {
      console.log(description)
      this.localPeerConnection.setLocalDescription(description)
    .then(() => {
      console.log(this.localPeerConnection.localDescription);
    }).catch(this.setSessionDescriptionError);
    },
    setSessionDescriptionError: function(err){
      console.log(err)
    },
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
      const localVideo = document.querySelector('#localVideo');
      const mediaStreamConstraints = {
        video: true,
      };
      navigator.mediaDevices.getUserMedia(mediaStreamConstraints)
      .then(mediaStream => {
        this.localStream = mediaStream;
        localVideo.srcObject = mediaStream;
      }).catch(error => {
        console.log('navigator.getUserMedia error: ', error);
      });
      this.localPeerConnection = new RTCPeerConnection();
      this.localPeerConnection.addStream(this.localStream);
      this.localPeerConnection.createOffer()
  .then(this.createdOffer).catch(this.setSessionDescriptionError);

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
