<script setup lang="ts">
import { Link } from '@inertiajs/inertia-vue3';
import { inject, h, Component, Ref } from 'vue';

import { NMenu } from 'naive-ui';

import { NIcon } from 'naive-ui';
import type { MenuOption } from 'naive-ui';
import { DashboardFilled } from '@vicons/material';
import { Person } from '@vicons/ionicons5';
import {
    PersonCall20Filled,
    Box20Filled,
    PersonBoard20Filled,
    ArrowCircleDown24Filled,
    ArrowCircleUp24Filled,
} from '@vicons/fluent';
import { Category } from '@vicons/carbon';
import { MoneyCheckAlt } from '@vicons/fa';

import route from 'ziggy-js';

function renderIcon(icon: Component) {
    return () => h(NIcon, null, { default: () => h(icon) });
}

function renderLink(url: string, content: string) {
    return () => h(Link, { href: url }, () => content);
}

const menuOptions: MenuOption[] = [
    {
        label: renderLink(route('admin.home'), 'Dashboard'),
        key: 'dashboard',
        icon: renderIcon(DashboardFilled),
    },
    {
        label: 'Pengguna',
        key: 'user',
        icon: renderIcon(Person),
        children: [
            {
                label: renderLink(route('admin.users.index'), 'Data Pengguna'),
                key: 'user-data',
                icon: renderIcon(Person),
            },
            {
                label: renderLink(
                    route('admin.divisions.index'),
                    'Bagian Pengguna'
                ),
                key: 'division-data',
                icon: renderIcon(PersonBoard20Filled),
            },
        ],
    },
    {
        label: renderLink(route('admin.suppliers.index'), 'Data Supplier'),
        key: 'supplier-data',
        icon: renderIcon(PersonCall20Filled),
    },
    {
        label: 'Barang',
        key: 'item',
        icon: renderIcon(Box20Filled),
        children: [
            {
                label: renderLink(route('admin.items.index'), 'Data Barang'),
                key: 'item-data',
                icon: renderIcon(Box20Filled),
            },
            {
                label: renderLink(
                    route('admin.items.categories.index'),
                    'Kategori Barang'
                ),
                key: 'item-category-data',
                icon: renderIcon(Category),
            },
        ],
    },
    {
        label: 'Transaksi',
        key: 'transaction',
        icon: renderIcon(MoneyCheckAlt),
        children: [
            {
                label: renderLink(route('admin.deposit.index'), 'Simpan'),
                key: 'deposit',
                icon: renderIcon(ArrowCircleDown24Filled),
            },
            {
                label: renderLink(route('stub'), 'Tarik'),
                key: 'withdraw',
                icon: renderIcon(ArrowCircleUp24Filled),
            },
        ],
    },
];

defineProps<{ active: string }>();
const collapsed = inject('sidebarCollapsed') as Ref<boolean> | undefined;
</script>
<template>
    <n-menu
        v-model:value="active"
        :options="menuOptions"
        :collapsed-width="64"
        :collapsed="collapsed" />
</template>