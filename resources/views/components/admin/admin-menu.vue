<script setup lang="ts">
import { Link } from '@inertiajs/inertia-vue3';
import { inject, h, Component, Ref } from 'vue';

import { NMenu } from 'naive-ui';

import { NIcon } from 'naive-ui';
import type { MenuOption } from 'naive-ui';
import {
    DashboardFilled,
    BadgeFilled,
    AssignmentFilled,
    SavingsFilled,
} from '@vicons/material';
import { Person, Settings } from '@vicons/ionicons5';
import {
    ArrowCircleDown24Filled,
    ArrowCircleUp24Filled,
    MoneyHand24Filled,
} from '@vicons/fluent';
import { MoneyCheckAlt, MoneyBillWave } from '@vicons/fa';

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
        label: 'Anggota',
        key: 'user',
        icon: renderIcon(Person),
        children: [
            {
                label: renderLink(route('admin.users.index'), 'Data Anggota'),
                key: 'user-data',
                icon: renderIcon(BadgeFilled),
            },
            {
                label: renderLink(
                    route('admin.savings.index'),
                    'Data Tabungan'
                ),
                key: 'saving-data',
                icon: renderIcon(SavingsFilled),
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
                label: renderLink(route('admin.withdraw.index'), 'Tarik'),
                key: 'withdraw',
                icon: renderIcon(ArrowCircleUp24Filled),
            },
            {
                label: renderLink(route('admin.loan.index'), 'Pinjam'),
                key: 'loan',
                icon: renderIcon(MoneyBillWave),
            },
            {
                label: renderLink(route('admin.installment.index'), 'Angsuran'),
                key: 'installment',
                icon: renderIcon(MoneyHand24Filled),
            },
        ],
    },
    {
        label: 'Laporan',
        key: 'report',
        icon: renderIcon(AssignmentFilled),
        children: [
            {
                label: renderLink(
                    route('admin.transactions.deposit'),
                    'Transaksi Simpan'
                ),
                key: 'deposit-data',
                icon: renderIcon(ArrowCircleDown24Filled),
            },
            {
                label: renderLink(
                    route('admin.transactions.withdraw'),
                    'Transaksi Tarik'
                ),
                key: 'withdraw-data',
                icon: renderIcon(ArrowCircleUp24Filled),
            },
            {
                label: renderLink(
                    route('admin.transactions.loan'),
                    'Transaksi Pinjam'
                ),
                key: 'loan-data',
                icon: renderIcon(MoneyBillWave),
            },
            {
                label: renderLink(
                    route('admin.transactions.installment'),
                    'Transaksi Angsuran'
                ),
                key: 'installment-data',
                icon: renderIcon(MoneyHand24Filled),
            },
        ],
    },
    {
        label: renderLink(route('admin.settings.index'), 'Pengaturan Koperasi'),
        key: 'settings',
        icon: renderIcon(Settings),
    },
];

const props = defineProps<{ active: string }>();

const collapsed = inject('sidebarCollapsed') as Ref<boolean> | undefined;
</script>
<template>
    <n-menu
        v-model:value="props.active"
        :options="menuOptions"
        :collapsed-width="64"
        :collapsed="collapsed" />
</template>
