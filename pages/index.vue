<template>
  <div class="container text-center">
    <div class="mt-5">
      <Logo />
      <h1 class="title p-5">NUXT Firebase Cloud Messaging</h1>
      <!--
      
      <b-button
        variant="danger"
        :disabled="listenersStarted"
        @click="startListeners"
        ><span class="font-size">Run FCM Service</span></b-button
      >
      <b-button
        variant="info"
        :disabled="!listenersStarted"
        @click="sendNotification"
        ><span class="font-size">Send Test Notification</span></b-button
      >
      -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
// var axios = require('axios')
export default {
  data() {
    return {
      listenersStarted: false,
      idToken: "",
    };
  },
  mounted() {
    this.startListeners();
  },
  methods: {
    async startListeners() {
      await this.startOnMessageListener();
      await this.startTokenRefreshListener();
      await this.requestPermission();
      await this.getIdToken();
      this.listenersStarted = true;
    },
    startOnMessageListener() {
      try {
        this.$fire.messaging.onMessage((payload) => {
          console.info("Message received : ", payload);
        });
      } catch (e) {
        console.error("Error : ", e);
      }
    },
    startTokenRefreshListener() {
      try {
        this.$fire.messaging.onTokenRefresh(async () => {
          try {
            await this.$fire.messaging.getToken();
          } catch (e) {
            console.error("Error : ", e);
          }
        });
      } catch (e) {
        console.error("Error : ", e);
      }
    },
    async requestPermission() {
      try {
        const permission = await Notification.requestPermission();
        console.log(permission);
      } catch (e) {
        console.error("Error : ", e);
      }
    },
    async getIdToken() {
      try {
        this.idToken = await this.$fire.messaging.getToken();
        console.log("TOKEN ID FROM FCM");
        console.log(this.idToken);
      } catch (e) {
        console.error("Error : ", e);
      }
    },
  },
};
</script>
<style>
.icon-size {
  font-size: 14px;
}
.font-size {
  font-size: 14px;
}
</style>
