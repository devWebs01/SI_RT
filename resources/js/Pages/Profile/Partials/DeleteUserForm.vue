<script setup>
import { useForm } from '@inertiajs/vue3';
import { nextTick, ref } from 'vue';

const confirmingUserDeletion = ref(false);
const passwordInput = ref(null);

const form = useForm({
    password: '',
});

const confirmUserDeletion = () => {
    confirmingUserDeletion.value = true;

    nextTick(() => passwordInput.value.focus());
};

const deleteUser = () => {
    form.delete(route('profile.destroy'), {
        preserveScroll: true,
        onSuccess: () => closeModal(),
        onError: () => passwordInput.value.focus(),
        onFinish: () => form.reset(),
    });
};

const closeModal = () => {
    confirmingUserDeletion.value = false;

    form.clearErrors();
    form.reset();
};
</script>

<template>
    <div>
        <header class="mb-3">
            <h5 class="mb-1">Delete Account</h5>
            <p class="text-muted small mb-0">
                Once your account is deleted, all of its resources and data will
                be permanently deleted. Before deleting your account, please
                download any data or information that you wish to retain.
            </p>
        </header>

        <button type="button" class="btn btn-danger" @click="confirmUserDeletion">
            Delete Account
        </button>

        <!-- Bootstrap Modal -->
        <div class="modal fade" :class="{ show: confirmingUserDeletion }"
            :style="{ display: confirmingUserDeletion ? 'block' : 'none' }" tabindex="-1" @click.self="closeModal">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">Delete Account</h5>
                        <button type="button" class="btn-close" @click="closeModal"></button>
                    </div>
                    <div class="modal-body">
                        <p>Are you sure you want to delete your account?</p>
                        <p class="text-muted small">
                            Once your account is deleted, all of its resources and data
                            will be permanently deleted. Please enter your password to
                            confirm you would like to permanently delete your account.
                        </p>
                        <div class="mt-3">
                            <label for="delete_password" class="form-label">Password</label>
                            <input id="delete_password" ref="passwordInput" v-model="form.password" type="password"
                                class="form-control" placeholder="Enter your password" @keyup.enter="deleteUser" />
                            <div v-if="form.errors.password" class="text-danger small mt-1">
                                {{ form.errors.password }}
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" @click="closeModal">
                            Cancel
                        </button>
                        <button type="button" class="btn btn-danger" :class="{ 'opacity-25': form.processing }"
                            :disabled="form.processing" @click="deleteUser">
                            Delete Account
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Backdrop -->
        <div v-if="confirmingUserDeletion" class="modal-backdrop fade show"></div>
    </div>
</template>
