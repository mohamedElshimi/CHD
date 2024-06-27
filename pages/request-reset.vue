<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <Head>
      <Title>CHD | Reset Password</Title>
    </Head>
    <div class="bg-white md:w-6/12 p-8 rounded shadow-md w-full max-w-md">
      <h2 class="text-2xl font-bold mb-6">Reset Password</h2>
      <form @submit.prevent="requestReset">
        <div class="mb-4">
          <label for="email" class="block text-gray-700">Email</label>
          <input
            type="email"
            v-model="email"
            class="mt-1 p-2 w-full border rounded"
            @blur="validateEmail"
            required
          />
          <span v-if="emailError" class="text-red-500">{{ emailError }}</span>
        </div>
        <div v-if="successMessage" class="mb-4 text-green-500">
          {{ successMessage }}
        </div>
        <div v-if="errorMessage" class="mb-4 text-red-500">
          {{ errorMessage }}
        </div>
        <button
          type="submit"
          class="w-full bg-primary text-white p-2 rounded"
          :disabled="isFormInvalid"
        >
          Send Reset Link
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// Intialize All Variables
const email = ref("");
const emailError = ref("");
const successMessage = ref("");
const errorMessage = ref("");
const base_url = ref("");

// Function To Check If Email is valid
const validateEmail = () => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  emailError.value = !emailPattern.test(email.value)
    ? "Invalid email format"
    : "";
};

// Function to Check Validate Of Form
const isFormInvalid = computed(() => !email.value || !!emailError.value);

// Function To Request Reset To get Code Or Token Depend on decumention
const requestReset = async () => {
  if (isFormInvalid.value) {
    return;
  }

  successMessage.value = "";
  errorMessage.value = "";
  try {
    await $fetch(base_url + "/api/mgr/auth/request-reset", {
      method: "POST",
      body: { email: email.value },
    });
    successMessage.value = "A reset Code has been sent to your email.";
  } catch (err) {
    errorMessage.value = "Failed to send reset Code. Please try again.";
  }
};
</script>
