<script setup lang="ts">
import { Head } from '@inertiajs/inertia-vue3';
import UserLayout from '@/views/layouts/user-layout.vue';
import Navbar from '@/views/components/navbar/navbar.vue';
import UserMenu from '@/views/components/user/user-menu.vue';

import { useAuth } from '@/scripts/composables/auth';

import { NH1, NSpace, NCard, NText, NTag, NButton } from 'naive-ui';

const active = 'deposit';

const props = defineProps<{
    type: string;
    amount: number;
    snapToken: string;
    midtransClientKey: string;
}>();

const auth = useAuth();

const submit = () => {
    // @ts-ignore
    window.snap.pay(props.snapToken);
};
</script>
<template layout="default">
    <component
        is="script"
        :data-client-key="midtransClientKey"
        src="https://app.sandbox.midtrans.com/snap/snap.js"
        async></component>
    <Head>
        <title>Pembayaran</title>
    </Head>
    <user-layout>
        <template #header>
            <navbar />
        </template>
        <template #sidebar>
            <user-menu :active="active"></user-menu>
        </template>
        <template #default>
            <n-space
                vertical
                class="w-96 mx-auto">
                <n-h1 align="center">Pembayaran</n-h1>
                <n-card>
                    <n-space
                        vertical
                        align="center"
                        justify="center">
                        <div class="flex flex-col items-center">
                            <h3 class="font-bold text-xl">Transaksi Simpan</h3>
                        </div>
                    </n-space>
                    <n-space
                        vertical
                        align="center"
                        justify="center">
                        <n-text depth="3">User ID: {{ auth.user.id }}</n-text>
                        <n-tag :bordered="false">{{ type }}</n-tag>
                    </n-space>
                    <n-space vertical>
                        <div class="flex flex-col">
                            <n-text>Nama Anggota:</n-text>
                            <n-text>{{ auth.user.name }}</n-text>
                        </div>
                        <div class="flex flex-col">
                            <n-text>Email Anggota:</n-text>
                            <n-text>{{ auth.user.email }}</n-text>
                        </div>
                        <div class="flex flex-col">
                            <n-text>Jenis Simpanan:</n-text>
                            <n-text>{{ type }}</n-text>
                        </div>
                        <div class="flex flex-col">
                            <n-text>Jumlah Simpanan:</n-text>
                            <n-text>
                                Rp.
                                {{ amount.toLocaleString('id-ID') }}
                            </n-text>
                        </div>
                    </n-space>
                    <template #action>
                        <n-button
                            @click="submit"
                            type="primary"
                            block
                            >Bayar</n-button
                        >
                    </template>
                </n-card>
            </n-space>
        </template>
    </user-layout>
</template>
