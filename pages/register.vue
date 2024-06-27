<template>
  <div class="min-h-screen bg-gray-200 flex justify-center items-center">
    <Head>
      <Title>CHD | Register</Title>
    </Head>
    <div class="w-11/12 mx-auto flex p-5">
      <div class="w-6/12 mx-auto hidden md:block">
        <img
          src="/assets/images/login.png"
          width="500"
          class="mx-auto"
          alt=""
        />
      </div>
      <div class="md:w-6/12 w-full mx-auto bg-white shadow-md rounded-md p-5">
        <h3 class="text-xl font-semibold text-secondary">Welcome to</h3>
        <h1 class="text-primary text-3xl font-bold">Cairo House Development</h1>
        <h3 class="text-center my-3 text-3xl font-semibold">Register</h3>
        <form action="" @submit.prevent="handleRegister">
          <!-- Name -->
          <div class="">
            <label for="" class="block text-gray-700 font-bold mb-2"
              >Name</label
            >
            <input
              type="text"
              id="name"
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline focus:shadow-primary"
              placeholder="Enter your Name"
              required
            />
          </div>

          <!-- Email -->
          <div class="my-3">
            <label for="" class="block text-gray-700 font-bold mb-2"
              >Email</label
            >
            <input
              type="email"
              id="name"
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline focus:shadow-primary"
              placeholder="Enter your Email"
              required
            />
          </div>

          <!-- Password -->
          <div class="my-3">
            <label for="" class="block text-gray-700 font-bold mb-2"
              >Password</label
            >
            <input
              type="password"
              id="name"
              class="input"
              placeholder="Enter your Password"
              required
            />
          </div>

          <button class="btn w-6/12 mx-auto block">Register</button>

          <!-- Error message -->
          <div v-if="error" class="my-4 text-center font-semibold text-red-500">
            {{ error }}
          </div>

          <!-- Loader -->
          <div
            v-if="isLoading"
            class="my-4 text-center font-semibold text-blue-500"
          >
            Loading ...
          </div>
        </form>
        <hr class="my-5" />

        <!-- Link to Login -->
        <h3 class="text-center font-semibold">
          Already have an account
          <NuxtLink to="/" class="text-primary">Login</NuxtLink>
        </h3>
      </div>
    </div>
  </div>
</template>

<script setup>
// import Liberaries
import { ref } from "vue";
import { useRouter } from "vue-router";

// Intialize Variables
const name = ref("");
const email = ref("");
const password = ref("");
const error = ref("");
const base_url = ref("");
const isLoading = ref(false);
const router = useRouter();

// Function to handle register request
const handleRegister = async () => {
  isLoading.value = true;
  error.value = "";
  try {
    await $fetch(base_url + "/api/mgr/auth/register", {
      method: "POST",
      body: { name: name.value, email: email.value, password: password.value },
    });
    // Store token
    localStorage.setItem("authToken", response.token);
    isLoading.value = false;
    router.push("/dashboard");
  } catch (err) {
    error.value = "Registration failed";
    isLoading.value = false;
  }
};
</script>

<style lang="scss" scoped></style>
