<script setup>
import Checkbox from '@/Components/Checkbox.vue';
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';

defineProps({
    canResetPassword: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const form = useForm({
    email: '',
    password: '',
    remember: false,
});

const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};
</script>

<template>
    <GuestLayout>

        <Head title="Log in" />

        <div v-if="status">
            {{ status }}
        </div>

        <form @submit.prevent="submit" class="form w-100" novalidate="novalidate" id="kt_sign_in_form">

            <!--begin::Heading-->
            <div class="text-center mb-10">
                <!--begin::Title-->
                <h1 class="text-dark mb-3">Inicio de Sesión</h1>
                <!--end::Title-->
                <!--begin::Link-->
                <div class="text-gray-400 fw-bold fs-4">Eres nuevo acá?
                    <a href="/register" class="link-primary fw-bolder">Registrate</a>
                </div>
                <!--end::Link-->
            </div>

            <div class="fv-row mb-10">
                <InputLabel for="email" value="Email: " />

                <TextInput id="email" type="email" class="mt-1 block w-full" v-model="form.email" required autofocus
                    autocomplete="username" />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div class="fv-row mb-10">
                <InputLabel for="password" value="Password: " />

                <TextInput id="password" type="password" class="mt-1 block w-full" v-model="form.password" required
                    autocomplete="current-password" />

                <InputError class="mt-2" :message="form.errors.password" />
            </div>

            <div class="my-5">
                <label class="flex items-center">
                    <Checkbox name="remember" v-model:checked="form.remember" />
                    <span class="ms-3 text-sm text-gray-600">Remember me</span>
                </label>
            </div>

            <div class="fv-row mb-10 text-center">
                <PrimaryButton class="btn-lg w-100 mb-3" :class="{ 'opacity-25': form.processing }"
                    :disabled="form.processing">
                    <span v-if="!form.processing" class="indicator-label"> Login </span>
                    <span v-else>
                        Please wait... <span class="spinner-border spinner-border-sm align-middle ms-2"></span>
                    </span>
                </PrimaryButton>

                <Link v-if="canResetPassword" :href="route('password.request')" class="link-primary fw-bolder">
                Forgot your password?
                </Link>
            </div>
        </form>
    </GuestLayout>
</template>
