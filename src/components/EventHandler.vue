<template>
</template>

<script>
import SockJS from "sockjs-client";
import Stomp from "webstomp-client";
import Constants from '../constants'

export default {
  data() {
    return {
    };
  },
  methods: {
    connect() {
      this.socket = new SockJS(Constants.SERVER_SOCKET_LINK);
      this.stompClient = Stomp.over(this.socket);
      this.stompClient.connect(
        {},
        frame => {
          this.stompClient.subscribe("/topic/push-messages", msg => {
            console.log(msg);
            var data = JSON.parse(msg.body);
            console.log(data);
            this.$swal(  'Success!',data.message,'success');
          });
        },
        error => {
          console.log(error);
        }
      );
    }
  },
  mounted() {
    this.connect();
  }
};
</script>

<style scoped>

</style>