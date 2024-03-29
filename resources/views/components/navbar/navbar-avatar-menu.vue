<script setup lang="ts">
import { Link } from '@inertiajs/inertia-vue3';
import { inject, Ref } from 'vue';
import { useAuth } from '@/scripts/composables/auth';
import { setTheme } from '@/scripts/composables/theme';
import { Moon, Sunny, Person, LogOut, LogIn } from '@vicons/ionicons5';
import { AdminPanelSettingsFilled } from '@vicons/material';
import {
    NTag,
    NIcon,
    NCard,
    NSpace,
    NButton,
    NAvatar,
    NDivider,
    GlobalTheme,
} from 'naive-ui';
import route from 'ziggy-js';

const theme: Ref<GlobalTheme> | undefined = inject('theme');
const auth = useAuth();
function toggleTheme() {
    setTheme(theme);
}
</script>
<template>
    <n-card content-style="min-width: 20rem;">
        <n-space
            vertical
            align="center"
            justify="center">
            <n-avatar
                bordered
                :size="72"
                object-fit="cover"
                round
                :src="auth.user?.picture ?? '/images/default-profile.jpg'" />
            <div
                v-if="auth.user"
                class="flex flex-col items-center">
                <h3 class="font-bold text-xl">{{ auth.user.name }}</h3>
                <h4 class="font-bold mb-2">{{ auth.user.email }}</h4>
                <n-tag :bordered="false">{{ auth.user.role }}</n-tag>
            </div>
            <div
                v-else
                class="flex flex-col items-center">
                <h3 class="font-bold text-xl">Guest</h3>
            </div>
        </n-space>
        <n-divider />
        <n-space vertical>
            <Link
                v-if="auth.user?.role === 'admin' ?? false"
                :href="route('admin.home')">
                <n-button
                    style="justify-content: left"
                    size="large"
                    strong
                    block
                    quaternary>
                    <template #icon>
                        <n-icon>
                            <admin-panel-settings-filled />
                        </n-icon>
                    </template>
                    Admin
                </n-button>
            </Link>
            <Link
                v-if="auth.user"
                :href="route('account.home')">
                <n-button
                    style="justify-content: left"
                    size="large"
                    strong
                    block
                    quaternary>
                    <template #icon>
                        <n-icon>
                            <person />
                        </n-icon>
                    </template>
                    Akun
                </n-button>
            </Link>
            <n-button
                style="justify-content: left"
                size="large"
                strong
                block
                quaternary
                @click="toggleTheme">
                <template #icon>
                    <n-icon>
                        <sunny v-if="theme?.name === 'light'" />
                        <moon v-else />
                    </n-icon>
                </template>
                Tema
            </n-button>
        </n-space>
        <n-divider />
        <n-space vertical>
            <Link
                v-if="auth.user"
                as="button"
                class="w-full"
                :href="route('signout')"
                method="post">
                <n-button
                    style="justify-content: left"
                    size="large"
                    strong
                    block
                    quaternary>
                    <template #icon>
                        <n-icon>
                            <log-out />
                        </n-icon>
                    </template>
                    Keluar
                </n-button>
            </Link>
            <Link
                v-else
                as="button"
                class="w-full"
                :href="route('signin')">
                <n-button
                    style="justify-content: left"
                    size="large"
                    strong
                    block
                    quaternary>
                    <template #icon>
                        <n-icon>
                            <log-in />
                        </n-icon>
                    </template>
                    Masuk
                </n-button>
            </Link>
        </n-space>
    </n-card>
</template>
