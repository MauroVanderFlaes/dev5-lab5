<script setup>
import { ref, reactive, onMounted } from 'vue';

let message = ref("");
let allMessages = reactive({ data: [] });

onMounted(async () => {
  const response = await fetch("https://lab5-p379.onrender.com/api/v1/messages/");
  const data = await response.json();
  allMessages.data = data;
});

const sendMessage = () => {
  allMessages.data.unshift({ user: "Mauro", text: message.value });
  fetch("https://lab5-p379.onrender.com/api/v1/messages/", {
    method: "POST",
    body: JSON.stringify({
      user: "Mauro",
      text: message.value,
    }),

    headers: {
      "Content-type": "application/json; charset=UTF-8",
    },
  })
    .then((response) => response.json())
    .then((json) => console.log(json));

  // clear input
  message.value = "";

};
</script>

<template>
  <ul>
    <li v-for="m in allMessages.data">
      <strong>{{ m.user }}:</strong> {{ m.text }}
    </li>
  </ul>
  <div>
    <input v-model="message" type="text" placeholder="" />
    <button @click="sendMessage">Send</button>
  </div>
</template>

<style scoped>
/* Add your styles here if needed */
li {
  list-style-type: none;
}
</style>
