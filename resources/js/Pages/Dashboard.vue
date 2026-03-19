<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
</script>

<template>

    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="fw-bold">Dashboard</h2>
        </template>

        <!-- Dashboard Content -->
        <div class="row">
            <!-- Cards -->
            <div class="col-lg-3 col-md-6 col-12 mb-4">
                <div class="card">
                    <div class="card-body">
                        <div class="d-flex justify-content-between align-items-center">
                            <div>
                                <p class="mb-0">Total Orders</p>
                                <h4 class="mb-0">8,234</h4>
                            </div>
                            <div class="avatar bg-label-primary rounded p-2">
                                <i class="bx bx-cart bx-sm"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 col-12 mb-4">
                <div class="card">
                    <div class="card-body">
                        <div class="d-flex justify-content-between align-items-center">
                            <div>
                                <p class="mb-0">Total Revenue</p>
                                <h4 class="mb-0">$48,250</h4>
                            </div>
                            <div class="avatar bg-label-success rounded p-2">
                                <i class="bx bx-dollar bx-sm"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 col-12 mb-4">
                <div class="card">
                    <div class="card-body">
                        <div class="d-flex justify-content-between align-items-center">
                            <div>
                                <p class="mb-0">New Customers</p>
                                <h4 class="mb-0">1,234</h4>
                            </div>
                            <div class="avatar bg-label-warning rounded p-2">
                                <i class="bx bx-user bx-sm"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 col-12 mb-4">
                <div class="card">
                    <div class="card-body">
                        <div class="d-flex justify-content-between align-items-center">
                            <div>
                                <p class="mb-0">Products</p>
                                <h4 class="mb-0">567</h4>
                            </div>
                            <div class="avatar bg-label-info rounded p-2">
                                <i class="bx bx-box bx-sm"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="row">
            <!-- Welcome Card -->
            <div class="col-12">
                <div class="card">
                    <div class="card-body">
                        <h4>Welcome back, {{ $page.props.auth.user.name }}!</h4>
                        <p class="mb-0">You're logged in successfully. Here's what's happening with your account today.
                        </p>
                    </div>
                </div>
            </div>
        </div>

        <div class="row mt-4">
            <!-- Recent Activity -->
            <div class="col-md-6 mb-4">
                <div class="card h-100">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <h5 class="mb-0">Recent Activity</h5>
                    </div>
                    <div class="card-body">
                        <ul class="list-unstyled mb-0">
                            <li class="d-flex mb-3">
                                <div class="avatar me-3">
                                    <span class="avatar-initial rounded bg-label-success"><i
                                            class="bx bx-check"></i></span>
                                </div>
                                <div>
                                    <h6 class="mb-0">Order #12345 completed</h6>
                                    <small class="text-muted">5 minutes ago</small>
                                </div>
                            </li>
                            <li class="d-flex mb-3">
                                <div class="avatar me-3">
                                    <span class="avatar-initial rounded bg-label-primary"><i
                                            class="bx bx-user-plus"></i></span>
                                </div>
                                <div>
                                    <h6 class="mb-0">New user registered</h6>
                                    <small class="text-muted">15 minutes ago</small>
                                </div>
                            </li>
                            <li class="d-flex">
                                <div class="avatar me-3">
                                    <span class="avatar-initial rounded bg-label-warning"><i
                                            class="bx bx-star"></i></span>
                                </div>
                                <div>
                                    <h6 class="mb-0">New review received</h6>
                                    <small class="text-muted">1 hour ago</small>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>

            <!-- Quick Actions -->
            <div class="col-md-6 mb-4">
                <div class="card h-100">
                    <div class="card-header d-flex justify-content-between align-items-center">
                        <h5 class="mb-0">Quick Actions</h5>
                    </div>
                    <div class="card-body">
                        <div class="d-grid gap-2">
                            <button class="btn btn-primary" type="button">
                                <i class="bx bx-plus me-1"></i> Add New Product
                            </button>
                            <button class="btn btn-outline-primary" type="button">
                                <i class="bx bx-file me-1"></i> View Reports
                            </button>
                            <button class="btn btn-outline-primary" type="button">
                                <i class="bx bx-cog me-1"></i> Settings
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
