<script setup lang="ts">
import { Link } from '@inertiajs/inertia-vue3';
import { ref, Ref, inject, CSSProperties } from 'vue';
import KoperasiLogo from '@/views/components/koperasi-logo.vue';
import SignInCard from '@/views/components/signin/signin-card.vue';
import SignInForm from '@/views/components/signin/signin-form.vue';
import SignInIllustration from '@/views/components/signin/signin-illustration.vue';
import { NTab, NTabs, NCard, NSpace, NElement } from 'naive-ui';
import route from 'ziggy-js';

const tab = ref('signin');

const title: Ref<string> | undefined = inject('title');

const tabsStyle: CSSProperties = {
    '--n-tab-gap': '20px',
    '--n-tab-font-size': '16px',
};

if (title) {
    title.value = tab.value === 'signin' ? 'Masuk' : 'Daftar';
}
</script>
<template>
    <sign-in-card>
        <n-element>
            <n-space
                class="p-8"
                justify="space-between"
                align="center">
                <Link :href="route('home')">
                    <koperasi-logo />
                </Link>
                <n-tabs
                    v-model:value="tab"
                    :style="tabsStyle"
                    default-value="signin"
                    animated>
                    <n-tab name="signin">
                        <span class="font-bold">Masuk</span>
                    </n-tab>
                    <n-tab name="signup">
                        <span class="font-bold">Daftar</span>
                    </n-tab>
                </n-tabs>
            </n-space>
            <n-card :bordered="false">
                <sign-in-form :tab="tab" />
            </n-card>
        </n-element>
        <sign-in-illustration class="hidden lg:grid" />
    </sign-in-card>
</template>
