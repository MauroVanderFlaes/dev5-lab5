<script setup>
import { ref, reactive, onMounted } from 'vue';

let message = ref("");
let allMessages = reactive({ data: [] });

onMounted(async () => {
  const storedMessages = localStorage.getItem('messages');
  if (storedMessages) {
    allMessages.data = JSON.parse(storedMessages);
  } else {
    const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages/");
    const data = await response.json();
    allMessages.data = data;
  }
});

const sendMessage = () => {
  allMessages.data.push({ user: "user", text: message.value });

  // Update local storage with the new messages
  localStorage.setItem('messages', JSON.stringify(allMessages.data));

  // Clear the input field
  message.value = "";
};
</script>

<template>
  <ul>
    <li v-for="m in allMessages.data" :key="m.id">
      <strong>{{ m.user }}:</strong> {{ m.text }}
    </li>
  </ul>
  <div>
    <input v-model="message" type="text" placeholder=""/>
    <button @click="sendMessage">Send</button>
  </div>
</template>

<style scoped>
/* Add your styles here if needed */
</style>
