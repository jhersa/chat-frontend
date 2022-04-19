<script>
export default {
  data() {
    return {
      ws: null,
      serverUrl: "ws://localhost:5000/ws",
      messages: [],
      newMessage: "",
    };
  },

  methods: {
    connectToWebsocket() {
      this.ws = new WebSocket(this.serverUrl);
      this.ws.addEventListener("open", (event) => {
        this.onWebsocketOpen(event);
      });
      this.ws.addEventListener("message", (event) => {
        this.handleNewMessage(event);
      });
    },
    onWebsocketOpen() {
      console.log("Conectado al websocket");
    },

    handleNewMessage(event) {
      let data = event.data;
      data = data.split(/\r?\n/);
      for (let i = 0; i < data.length; i++) {
        let msg = JSON.parse(data[i]);
        this.messages.push(msg);
      }
    },

    sendMessage() {
      if (this.newMessage !== "") {
        this.ws.send(JSON.stringify({ message: this.newMessage }));
        this.newMessage = "";
      }
    },
  },

  mounted() {
    this.connectToWebsocket();
  },
};
</script>

<template>
  <div class="container-fluid h-100">
    <div class="row justify-content-center h-100">
      <div class="col-md-8 col-xl-6 chat">
        <div class="card">
          <div class="card-header msg_head">
            <div class="d-flex bd-highlight justify-content-center">Chat</div>
          </div>
          <div class="card-body msg_card_body">
            <div
              v-for="(message, key) in messages"
              :key="key"
              class="d-flex justify-content-start mb-4"
            >
              <div class="msg_cotainer">
                {{ message.message }}
                <span class="msg_time"></span>
              </div>
            </div>
          </div>
          <div class="card-footer">
            <div class="input-group">
              <textarea
                v-model="newMessage"
                name=""
                class="form-control type_msg"
                placeholder="Escribe tu mensaje..."
                @keyup.enter.exact="sendMessage"
              ></textarea>
              <div class="input-group-append">
                <span
                  class="input-group-text send_btn"
                  @click="sendMessage"
                ></span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
