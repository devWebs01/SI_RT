<script setup>
import { Link, useForm, usePage } from '@inertiajs/vue3';

defineProps({
    mustVerifyEmail: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const user = usePage().props.auth.user;

const form = useForm({
    name: user.name,
    email: user.email,
});
</script>

<template>
    <form @submit.prevent="form.patch(route('profile.update'))">
        <div class="mb-3">
            <label for="name" class="form-label">Name</label>
            <input id="name" type="text" class="form-control" v-model="form.name" required autofocus
                autocomplete="name" />
            <div v-if="form.errors.name" class="text-danger small mt-1">
                {{ form.errors.name }}
            </div>
        </div>

        <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <input id="email" type="email" class="form-control" v-model="form.email" required autocomplete="username" />
            <div v-if="form.errors.email" class="text-danger small mt-1">
                {{ form.errors.email }}
            </div>
        </div>

        <div v-if="mustVerifyEmail && user.email_verified_at === null">
            <div class="alert alert-warning mb-3">
                Your email address is unverified.
                <Link :href="route('verification.send')" method="post" as="button" class="alert-link">
                    Click here to re-send the verification email.
                </Link>
            </div>

            <div v-if="status === 'verification-link-sent'" class="alert alert-success mb-3">
                A new verification link has been sent to your email address.
            </div>
        </div>

        <div class="d-flex align-items-center gap-2">
            <button type="submit" class="btn btn-primary" :disabled="form.processing">
                Save Changes
            </button>

            <span v-if="form.recentlySuccessful" class="text-success">
                Saved.
            </span>
        </div>
    </form>
</template>
