<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <Head>
      <Title>CHD | Set New Password</Title>
    </Head>
    <div class="bg-white p-8 rounded shadow-md w-full max-w-md">
      <h2 class="text-2xl font-bold mb-6">Set New Password</h2>
      <form @submit.prevent="resetPassword">
        <div class="mb-4">
          <label for="code" class="block text-gray-700">Code</label>
          <input type="text" v-model="code" class="input" required />
        </div>
        <div class="mb-4">
          <label for="password" class="block text-gray-700">New Password</label>
          <input
            type="password"
            v-model="password"
            @blur="validatePasswords"
            class="input"
            required
          />
        </div>
        <div class="mb-4">
          <label for="confirmPassword" class="block text-gray-700"
            >Confirm Password</label
          >
          <input
            type="password"
            v-model="confirmPassword"
            @blur="validatePasswords"
            class="input"
            required
          />

          <!-- Error Apper When Password Dosen't Match -->
          <span v-if="passwordError" class="text-red-500">{{
            passwordError
          }}</span>
        </div>

        <!-- Confirmation That Password Reset Successfully -->
        <div v-if="successMessage" class="mb-4 text-green-500">
          {{ successMessage }}
        </div>

        <!-- Error Message in Case Request Fail -->
        <div v-if="errorMessage" class="mb-4 text-red-500">
          {{ errorMessage }}
        </div>
        <button
          type="submit"
          class="w-full bg-primary text-white p-2 rounded"
          :disabled="isFormInvalid"
        >
          Set New Password
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter, useRoute } from "vue-router";

// Initialize Variables
const code = ref("");
const password = ref("");
const confirmPassword = ref("");
const passwordError = ref("");
const successMessage = ref("");
const errorMessage = ref("");
const router = useRouter();
const route = useRoute();

// Function to Check Match Of Password
const validatePasswords = () => {
  passwordError.value =
    password.value != confirmPassword.value ? "Passwords do not match" : "";
};

// Function to Check Form Validity
const isFormInvalid = computed(
  () => !password.value || !confirmPassword.value || !!passwordError.value
);

// Function of Request To Reset Password
const resetPassword = async () => {
  if (isFormInvalid.value) {
    return;
  }

  successMessage.value = "";
  errorMessage.value = "";
  try {
    await $fetch(`${base_url}/api/mgr/auth/reset-password`, {
      method: "POST",
      body: {
        password: password.value,
        confirmPassword: confirmPassword.value,
        code: code.value,
      },
    });
    successMessage.value = "Password reset successfully. You can now log in.";
    setTimeout(() => {
      router.push("/login");
    }, 2000);
  } catch (err) {
    errorMessage.value = "Failed to reset password. Please try again.";
  }
};
</script>
