<script setup>
import { ref } from 'vue';
import Print from './components/Print.vue';
import axiosInstance from './utils/axios';
import { getCookie } from 'awesome-cookie';


const fullName = ref('');
const email = ref('');
const password = ref('');
const passwordConfirmation = ref('');

const userDetails = ref(null);

const submit = async () => {
  const payload = {
    name: fullName.value,
    email: email.value,
    password: password.value,
    password_confirmation: passwordConfirmation.value,
  };

  await axiosInstance.get('/sanctum/csrf-cookie');
  await axiosInstance.post('/register', payload);

  const { data } = await axiosInstance.get('/api/user');

  userDetails.value = data;

}

</script>

<template>
  <div>
    <Print />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
