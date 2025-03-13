<script lang="ts" setup>
definePageMeta({
    middleware: ['sanctum:auth'],
});

useSeoMeta({
    title: 'Home',
});

const { user } = useSanctumAuth();
const { logout } = useSanctumAuth();
</script>

<template>
    <div class="p-4">
        <pre class="p-4 border border-gray-300 rounded w-fit">{{ user }}</pre>
        <button @click="logout" class="mt-4 p-2 bg-red-500 text-white rounded cursor-pointer">
            Logout
        </button>
    </div>
</template>

<style></style>