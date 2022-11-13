<script setup lang="ts">
import { ref } from 'vue';
import { Head, Link, useForm } from '@inertiajs/inertia-vue3';
import AdminMenu from '@/views/components/admin/admin-menu.vue';
import Navbar from '@/views/components/navbar/navbar.vue';
import AdminLayout from '@/views/layouts/admin-layout.vue';
import {
    DivisionProperties,
    UserProperties,
} from '@/scripts/composables/model';
import {
    NSpace,
    NButton,
    NH1,
    NForm,
    NCard,
    NFormItem,
    NSelect,
    FormRules,
    NInput,
    NAutoComplete,
    FormInst,
    NElement,
} from 'naive-ui';
import { SelectMixedOption } from 'naive-ui/es/select/src/interface';
import route from 'ziggy-js';

const props = defineProps<{
    divisions: DivisionProperties[];
    user: UserProperties;
}>();

const formRef = ref<FormInst | null>(null);

const form = useForm({
    name: props.user.name,
    email: props.user.email as string,
    password: '',
    role: props.user.role,
    division: props.user.division?.id,
    address: props.user.address,
    contact: props.user.contact,
});

const formRules: FormRules = {
    name: [
        {
            required: true,
            message: 'Nama diperlukan',
            trigger: ['input', 'blur'],
        },
    ],
    email: [
        {
            required: true,
            message: 'Email diperlukan',
            trigger: ['input', 'blur'],
        },
        {
            type: 'email',
            message: 'Hanya menerima email',
            trigger: 'blur',
        },
    ],
    password: [
        {
            min: 8,
            message: 'Minimal 8 karakter',
            trigger: ['input', 'blur'],
        },
        {
            max: 24,
            message: 'Maksimal 24 karakter',
            trigger: ['input', 'blur'],
        },
    ],
    role: [
        {
            required: true,
            message: 'Role diperlukan',
            trigger: 'blur',
        },
    ],
    division: [
        {
            type: 'number',
            required: true,
            message: 'Divisi diperlukan',
            trigger: 'blur',
        },
    ],
    address: [
        {
            required: true,
            message: 'Alamat diperlukan',
            trigger: ['input', 'blur'],
        },
    ],
    contact: [
        {
            required: true,
            message: 'Kontak diperlukan',
            trigger: ['input', 'blur'],
        },
    ],
};

const divisionOptions: SelectMixedOption[] = props.divisions.map((v) => {
    return {
        label: v.position,
        value: v.id,
    } as SelectMixedOption;
});

const roleOptions: SelectMixedOption[] = [
    {
        label: 'Anggota',
        value: 'anggota',
    },
    {
        label: 'Admin',
        value: 'admin',
    },
];

const emailOptions = (form: { email: string }) => {
    return [
        ['Google', '@gmail.com'],
        ['Yahoo', '@yahoo.com'],
        ['Outlook', '@outlook.com'],
    ].map((emailInfo) => {
        return {
            type: 'group',
            label: emailInfo[0],
            key: emailInfo[0],
            children: [form.email?.split('@')[0] + emailInfo[1]],
        };
    });
};

const submitForm = () => {
    formRef.value?.validate((errors) => {
        if (!errors) {
            form.put(route('admin.users.update', props.user.id));
        }
    });
};

const active = 'user-data';
</script>
<template layout="default">
    <Head>
        <title>Admin | Edit Pengguna</title>
    </Head>
    <admin-layout>
        <template #header>
            <navbar type="admin" />
        </template>
        <template #sidebar>
            <admin-menu :active="active"> </admin-menu>
        </template>
        <template #default>
            <n-space vertical>
                <n-h1 align="center">Edit Pengguna</n-h1>
                <Link :href="route('admin.users.index')">
                    <n-button type="primary">Kembali</n-button>
                </Link>
                <n-card>
                    <n-form
                        ref="formRef"
                        size="large"
                        :model="form"
                        :rules="formRules"
                        @submit.prevent="submitForm">
                        <n-form-item
                            path="name"
                            label="Nama">
                            <n-input
                                v-model:value="form.name"
                                placeholder="Nama Pengguna" />
                        </n-form-item>
                        <n-element
                            v-if="form.errors.name"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.name }}
                        </n-element>
                        <n-form-item
                            path="email"
                            label="Email">
                            <n-auto-complete
                                v-model:value="form.email"
                                :options="emailOptions(form)"
                                placeholder="Email Pengguna"
                                type="text">
                            </n-auto-complete>
                        </n-form-item>
                        <n-element
                            v-if="form.errors.email"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.email }}
                        </n-element>
                        <n-form-item
                            path="password"
                            label="Password Baru">
                            <n-input
                                v-model:value="form.password"
                                placeholder="Password Baru Pengguna"
                                show-password-on="click"
                                type="password" />
                        </n-form-item>
                        <n-element
                            v-if="form.errors.password"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.password }}
                        </n-element>
                        <n-form-item
                            path="division"
                            label="Divisi">
                            <n-select
                                v-model:value="form.division"
                                filterable
                                :options="divisionOptions"
                                placeholder="Divisi Pengguna" />
                        </n-form-item>
                        <n-element
                            v-if="form.errors.division"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.division }}
                        </n-element>
                        <n-form-item
                            path="role"
                            label="Role">
                            <n-select
                                v-model:value="form.role"
                                filterable
                                :options="roleOptions"
                                placeholder="Role Pengguna" />
                        </n-form-item>
                        <n-element
                            v-if="form.errors.role"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.role }}
                        </n-element>
                        <n-form-item
                            path="address"
                            label="Alamat">
                            <n-input
                                v-model:value="form.address"
                                placeholder="Alamat Pengguna" />
                        </n-form-item>
                        <n-element
                            v-if="form.errors.address"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.address }}
                        </n-element>
                        <n-form-item
                            path="contact"
                            label="Kontak">
                            <n-input
                                v-model:value="form.contact"
                                placeholder="Kontak Pengguna" />
                        </n-form-item>
                        <n-element
                            v-if="form.errors.contact"
                            class="text-[var(--error-color)] mb-6">
                            {{ form.errors.contact }}
                        </n-element>
                        <n-button
                            :disabled="form.processing"
                            :loading="form.processing"
                            attr-type="submit"
                            size="large"
                            block
                            type="success"
                            >Edit
                        </n-button>
                    </n-form>
                </n-card>
            </n-space>
        </template>
    </admin-layout>
</template>