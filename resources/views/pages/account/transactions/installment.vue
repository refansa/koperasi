<script setup lang="ts">
import { Head, Link } from '@inertiajs/inertia-vue3';
import { h } from 'vue';
import { InstallmentProperties } from '@/scripts/composables/model';
import UserMenu from '@/views/components/user/user-menu.vue';
import Navbar from '@/views/components/navbar/navbar.vue';
import UserLayout from '@/views/layouts/user-layout.vue';
import { NH1, NSpace, NDataTable, DataTableColumns, NButton } from 'naive-ui';
import route from 'ziggy-js';
import dayjs from 'dayjs';

defineProps<{ installments: InstallmentProperties[] }>();

const active = 'installment-data';

const createColumns = (): DataTableColumns<InstallmentProperties> => {
    return [
        {
            title: 'No Transaksi',
            key: 'id',
            sorter: 'default',
            render(u) {
                return `A-${u.id}`;
            },
        },
        {
            title: 'No Pinjam',
            key: 'loan.id',
            sorter: (a, b) => (a.loan?.id ?? 0) - (b.loan?.id ?? 0),
            render(u) {
                return `P-${u.loan?.id}`;
            },
        },
        {
            title: 'Angsuran',
            key: 'transaction.amount',
            sorter: (a, b) =>
                (a.transaction?.amount ?? 0) - (b.transaction?.amount ?? 0),
            render: (u) => {
                return `Rp. ${(u.transaction?.amount ?? 0).toLocaleString(
                    'id-ID'
                )}`;
            },
        },
        {
            title: 'Angsuran Ke',
            key: 'installment_of',
            sorter: (a, b) => a.installment_of - b.installment_of,
        },
        {
            title: 'Sisa Tenor',
            key: 'installment_left',
            sorter: (a, b) => a.installment_left - b.installment_left,
        },
        {
            title: 'Keterangan',
            key: 'note',
            sorter: 'default',
        },
        {
            title: 'Tanggal Transaksi',
            key: 'created_at',
            sorter: 'default',
            render(u) {
                return dayjs(u.created_at).format('DD MMM YYYY');
            },
        },
        {
            title: 'Aksi',
            key: 'actions',
            align: 'center',
            width: 100,
            render(u) {
                return h(
                    Link,
                    {
                        href: route('account.installment.receipt', u.id),
                    },
                    () => h(NButton, { type: 'primary' }, 'Lihat')
                );
            },
        },
    ];
};

const columns = createColumns();
</script>
<template layout="default">
    <Head>
        <title>Data Transaksi Angsuran</title>
    </Head>
    <user-layout>
        <template #header>
            <navbar />
        </template>
        <template #sidebar>
            <user-menu :active="active"> </user-menu>
        </template>
        <template #default>
            <n-space vertical>
                <n-h1 align="center">Data Transaksi Angsuran</n-h1>
                <n-data-table
                    :bordered="false"
                    :single-line="false"
                    :columns="columns"
                    :data="installments"
                    :scroll-x="1600"
                    :pagination="{ pageSize: 10 }" />
            </n-space>
        </template>
    </user-layout>
</template>
