<template>
  <header class="bg-slate-200">
    <Head>
      <Title>CHD | Dashboard</Title>
    </Head>
    <nav class="py-5 container mx-auto flex justify-between">
      <img src="/public/favicon.ico" alt="" />
      <button @click="logout" class="btn">Logout</button>
    </nav>
  </header>
  <h2>Dashboard</h2>
</template>

<script setup>
import { useRouter } from "vue-router";

const router = useRouter();

const logout = () => {

  // Remove token from Local Storage
  localStorage.removeItem("authToken");

  // Push to Login
  router.push("/");
};
</script>
