<script setup>
definePageMeta({
  middleware: 'redirect-if-auth'
})
import { ref } from 'vue'
const email = ref('')
const pass = ref('')
const passConfirm = ref('')
const client = useSupabaseClient()


async function register() {
  if (pass.value !== passConfirm.value) {
    return alert("Parolele nu se potrivesc")
  }

  const { data, error } = await client.auth.signUp({
    email: email.value,
    password: pass.value,
  })

  if (error) {
    return alert(error.message)
  }

  // Apelează API-ul tău pentru a salva profilul (ca în Firebase)
  await $fetch(`/api/proxy/profile`, {
    method: 'POST',
    body: {
      email: data.user.email,
      profileId: data.user.id,
      displayName: data.user.user_metadata?.full_name || '', // poți ajusta
    }
  })

  navigateTo('/confirm')
}
</script>


<template>
  <div class="w-screen flex justify-center items-center min-h-screen flex-col">
    <div class="login-box">
      <h2>Sign up</h2>
      <form @submit.prevent="register">
        <div class="user-box">
          <input type="email" name="" required="" v-model="email" />
          <label>Email</label>
        </div>
        <div class="user-box">
          <input type="password" name="" required="" v-model="pass" />
          <label>Parolă</label>
        </div>
        <div class="user-box">
          <input type="password" name="" required="" v-model="passConfirm" />
          <label>Confirmare parolă</label>
        </div>
        <a href="#" @click="register" class="login-button">
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          Sign Up
        </a>
        <input type="submit" hidden />
      </form>
    </div>
  </div>
</template>

<style scoped>
.styled-button {
  all: unset;
  cursor: pointer;
}
html {
  height: 100%;
}
body {
  margin: 0;
  padding: 0;
  background: linear-gradient(#141e30, #243b55);
}

.login-box {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 400px;
  padding: 40px;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.5);
  box-sizing: border-box;
  box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
  border-radius: 10px;
}

.login-box h2 {
  margin: 0 0 30px;
  padding: 0;
  color: #fff;
  text-align: center;
}

.login-box .user-box {
  position: relative;
}

.login-box .user-box input {
  width: 100%;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  margin-bottom: 30px;
  border: none;
  border-bottom: 1px solid #fff;
  outline: none;
  background: transparent;
}
.login-box .user-box label {
  position: absolute;
  top: 0;
  left: 0;
  padding: 10px 0;
  font-size: 16px;
  color: #fff;
  pointer-events: none;
  transition: 0.5s;
}

.login-box .user-box input:focus ~ label,
.login-box .user-box input:valid ~ label {
  top: -20px;
  left: 0;
  color: #03e9f4;
  font-size: 12px;
}

.login-box form a {
  position: relative;
  display: inline-block;
  padding: 10px 20px;
  color: #03e9f4;
  font-size: 16px;
  text-decoration: none;
  text-transform: uppercase;
  overflow: hidden;
  transition: 0.5s;
  letter-spacing: 4px;
}

.login-box a:hover {
  background: #03e9f4;
  color: #fff;
  border-radius: 5px;
  box-shadow: 0 0 5px #03e9f4, 0 0 25px #03e9f4, 0 0 50px #03e9f4,
  0 0 100px #03e9f4;
}

.login-box a span {
  position: absolute;
  display: block;
}

.login-box a span:nth-child(1) {
  top: 0;
  left: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, #03e9f4);
  animation: btn-anim1 1s linear infinite;
}

@keyframes btn-anim1 {
  0% {
    left: -100%;
  }
  50%,
  100% {
    left: 100%;
  }
}

.login-box a span:nth-child(2) {
  top: -100%;
  right: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(180deg, transparent, #03e9f4);
  animation: btn-anim2 1s linear infinite;
  animation-delay: 0.25s;
}

@keyframes btn-anim2 {
  0% {
    top: -100%;
  }
  50%,
  100% {
    top: 100%;
  }
}

.login-box a span:nth-child(3) {
  bottom: 0;
  right: -100%;
  width: 100%;
  height: 2px;
  background: linear-gradient(270deg, transparent, #03e9f4);
  animation: btn-anim3 1s linear infinite;
  animation-delay: 0.5s;
}

@keyframes btn-anim3 {
  0% {
    right: -100%;
  }
  50%,
  100% {
    right: 100%;
  }
}

.login-box a span:nth-child(4) {
  bottom: -100%;
  left: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(360deg, transparent, #03e9f4);
  animation: btn-anim4 1s linear infinite;
  animation-delay: 0.75s;
}

.additional {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  a {
    margin-top: 0;
  }
  .clear {
    display: flex;
    align-items: center;
    justify-content: center;
  }
}

@keyframes btn-anim4 {
  0% {
    bottom: -100%;
  }
  50%,
  100% {
    bottom: 100%;
  }
}
</style>