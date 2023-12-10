<script setup>
import { ref, reactive, onMounted } from 'vue';


let message = ref(""); //int, string, boolean
let allMessages = reactive({data: []}); //array, object



//fetch this api https://lab5-p379.onrender.com/api/v1/messages/ and put the "text" and "user" into allMessages.data
onMounted(async () => {
    const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages/");
    const data = await response.json();
    allMessages.data = data;
});







//function sendMessage
const sendMessage = () => {
    message.user = "user";
    message.value = "";
    allMessages.data.push(message.value);
};

</script>

<template>
  <ul>
    <li v-for="m in allMessages.data">
        <strong>{{ m.user }}:</strong> {{ m.text }}
      </li>
  </ul>
  <div>
    <input v-model="message" type="text" placeholder=""/>
    <button @click="sendMessage">Send</button>
  </div>
</template>

<style scoped>

</style>
