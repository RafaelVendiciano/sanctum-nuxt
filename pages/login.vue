<script lang="ts" setup>
    const form = ref({
        email:"test@test.com",
        password:"password",
    })

    async function handleLogin() {
        await $fetch("http://localhost:8000/sanctum/csrf-cookie", { 
            credentials: "include"
        })

        const token = useCookie('XSRF-TOKEN')
        

        await useFetch("http://localhost:8000/api/login", {
            credentials:"include",
            method: "POST",
            watch: false,
            body: form.value,
            headers: {
              'X-XSRF-TOKEN': token.value as string
            }
        })

        const response = useFetch("http://localhost:8000/api/user", {
            credentials:"include",
            watch: false,
            headers: {
              'X-XSRF-TOKEN': token.value as string
            }
        })

        console.log(response)

    }

</script>

<template>
  <div>
    Page: Login
    <br>
    <form @submit.prevent="handleLogin">
        <input name="email" v-model="form.email">
        <input name="password" v-model="form.password">

        <button>Login</button>
    </form>
  </div>
</template>

<style scoped></style>
