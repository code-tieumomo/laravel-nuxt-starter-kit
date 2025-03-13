<script lang="ts" setup>
import { toast } from 'vue-sonner';

definePageMeta({
  middleware: ['sanctum:guest'],
});

useSeoMeta({
  title: 'Register',
});

const { login } = useSanctumAuth();

interface Credentials {
  name: string;
  email: string;
  password: string;
  password_confirmation: string;
}

const credentials = ref<Credentials>({
  name: '',
  email: '',
  password: '',
  password_confirmation: '',
});

const inputName = useTemplateRef<HTMLInputElement>('inputName');
const inputEmail = useTemplateRef<HTMLInputElement>('inputEmail');
const inputPassword = useTemplateRef<HTMLInputElement>('inputPassword');
const inputPasswordConfirmation = useTemplateRef<HTMLInputElement>('inputPasswordConfirmation');

const registerHandler = async () => {
  try {
    const res: { id?: number } = await $fetch('http://localhost:8000/api/register', {
      method: 'POST',
      body: credentials.value,
    });

    if (res.id) {
      const res = await login(credentials.value);
    }
  } catch (error: any) {
    if (error.response?._data?.errors?.name) {
      inputName.value?.focus();
    } else if (error.response?._data?.errors?.email) {
      inputEmail.value?.focus();
    } else if (error.response?._data?.errors?.password) {
      inputPassword.value?.focus();
    } else if (error.response?._data?.errors?.password_confirmation) {
      inputPasswordConfirmation.value?.focus();
    }

    toast.error(error.response?._data?.message || error.message);
  }
};
</script>

<template>
  <main class="min-h-screen min-w-screen flex items-center justify-center">
    <div class="w-full max-w-md">
      <form class="w-full" @submit.prevent="registerHandler">
        <h1 class="text-2xl font-medium">Register</h1>

        <input ref="inputName" type="text" placeholder="Name" class="w-full mt-4 p-2 border border-gray-300 rounded"
          v-model="credentials.name" />
        <input ref="inputEmail" type="text" placeholder="Email" class="w-full mt-4 p-2 border border-gray-300 rounded"
          v-model="credentials.email" />
        <input ref="inputPassword" type="password" placeholder="Password"
          class="w-full mt-4 p-2 border border-gray-300 rounded" v-model="credentials.password" />
        <input ref="inputPasswordConfirmation" type="password" placeholder="Confirm Password"
          class="w-full mt-4 p-2 border border-gray-300 rounded" v-model="credentials.password_confirmation" />

        <button type="submit" class="w-full mt-4 p-2 bg-black text-white rounded cursor-pointer">Register</button>
      </form>

      <div class="mt-4 text-right">
        <NuxtLink to="/login" class="underline">Login</NuxtLink>
      </div>
    </div>
  </main>
</template>

<style></style>