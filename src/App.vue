<script>
export default {
  data() {
    return {
      ws: null,
      serverUrl: "ws://localhost:5000/ws",
    };
  },

  methods: {
    connectToWebsocket() {
      this.ws = new WebSocket(this.serverUrl);
      this.ws.addEventListener("open", (event) => {
        this.onWebsocketOpen(event);
      });
    },
    onWebsocketOpen() {
      console.log("Conectado al websocket");
    },
  },

  mounted() {
    this.connectToWebsocket();
  },
};
</script>

<template></template>
