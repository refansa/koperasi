<script setup lang="ts">
import { Head } from '@inertiajs/inertia-vue3';
import { ref, provide } from 'vue';
import SignInWrapper from '@/views/components/signin/signin-wrapper.vue';
import SignInBackground from '@/views/components/signin/signin-background.vue';

const title = ref('Masuk');

provide('title', title);
</script>
<template layout="default">
    <Head>
        <title>{{ title }}</title>
    </Head>
    <sign-in-background>
        <sign-in-wrapper />
    </sign-in-background>
</template>
