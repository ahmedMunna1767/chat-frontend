<template>
    <ChatComponent />
</template>


<script setup>
import { ref, onMounted } from "vue";

import ChatComponent from "@/components/ChatComponent.vue"

const msgArray = ref([]);
const userName = ref("Munna");

var socket = new WebSocket("ws://localhost:8080/ws");

const connect = () => {
  console.log("Attempting Connection...");

  socket.onopen = () => {
    console.log("Successfully Connected");
  };

  socket.onmessage = msg => {
    console.log(msg);
    msgArray.value.push(msg.data)
  };

  socket.onclose = event => {
    console.log("Socket Closed Connection: ", event);
  };

  socket.onerror = error => {
    console.log("Socket Error: ", error);
  };
};

const sendMsg = (msg) => {
  console.log("sending msg: ", msg);
  socket.send(msg);
};

onMounted(() => {
   connect();
});

</script>