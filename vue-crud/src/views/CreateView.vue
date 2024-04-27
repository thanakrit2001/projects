<template>
  <div class="q-gutter-md q-pa-md" style="max-width: 400px">
    <q-form @submit="onSubmit" class="q-gutter-md">
      <q-input v-model="fname" label="Firstname" />
      <q-input v-model="lname" label="Lastname" />
      <q-input v-model="username" label="Username" />
      <q-input v-model="password" label="Password" />
      <q-input v-model="email" label="Email" />
      <q-input v-model="avatar" label="Avatar" />
      <q-btn label="Submit" type="submit" color="primary" />
    </q-form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import router from '../router'

const fname = ref('Thanakrit')
const lname = ref('Cankha')
const username = ref('thanakrit')
const password = ref('1212312121')
const email = ref('thanakrit@gmail.com')
const avatar = ref('https://www.melivecode.com/users/cat.png')

const onSubmit = () => {
  const myHeaders = new Headers()
  myHeaders.append('Content-Type', 'application/json')

  const raw = JSON.stringify({
    fname: fname.value,
    lname: lname.value,
    username: username.value,
    password: password.value,
    email: email.value,
    avatar: avatar.value,
  })

  const requestOptions = {
    method: 'POST',
    headers: myHeaders,
    body: raw,
    redirect: 'follow',
  }

  fetch('https://www.melivecode.com/api/users/create', requestOptions)
    .then((response) => response.json())
    .then((result) => {
      alert(result.message)
      if (result.status === 'ok') {
        router.push('/')
      }
    })
    .catch((error) => console.error(error))
}
</script>
<style></style>
