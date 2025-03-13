<script lang="ts" setup>
import { toast } from 'vue-sonner';

definePageMeta({
  middleware: ['sanctum:guest'],
});

useSeoMeta({
  title: 'Login',
});

const { login } = useSanctumAuth();

interface Credentials {
  email: string;
  password: string;
  remember: boolean;
}

const credentials = ref<Credentials>({
  email: '',
  password: '',
  remember: false,
});

const inputEmail = useTemplateRef<HTMLInputElement>('inputEmail');
const inputPassword = useTemplateRef<HTMLInputElement>('inputPassword');

const loginHandler = async () => {
  try {
    await login(credentials.value);
  } catch (error: any) {
    if (error.response?._data?.errors.email) {
      inputEmail.value?.focus();
    } else if (error.response?._data?.errors.password) {
      inputPassword.value?.focus();
    }

    toast.error(error.response?._data?.message || error.message);
  }
};
</script>

<template>
  <main class="min-h-screen min-w-screen flex items-center justify-center">
    <div class="w-full max-w-md">
      <form class="w-full" @submit.prevent="loginHandler">
        <h1 class="text-2xl font-medium">Login</h1>

        <input ref="inputEmail" type="text" placeholder="Email" class="w-full mt-4 p-2 border border-gray-300 rounded"
          v-model="credentials.email" />
        <input ref="inputPassword" type="password" placeholder="Password"
          class="w-full mt-4 p-2 border border-gray-300 rounded" v-model="credentials.password" />
        <label class="flex items-center mt-4">
          <input type="checkbox" class="mr-2" v-model="credentials.remember" :true-value="true" :false-value="false" />
          <span>Remember me</span>
        </label>

        <button type="submit" class="w-full mt-4 p-2 bg-black text-white rounded cursor-pointer">Login</button>
      </form>

      <div class="mt-4 text-right">
        <NuxtLink to="/register" class="underline">Register</NuxtLink>
      </div>
    </div>
  </main>
</template>

<style></style>