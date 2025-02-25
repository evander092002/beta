<template>
  <div class="h-auto w-full flex flex-col p-5 gap-10">
    <section class="">
      <UBreadcrumb :links="links">
        <template #divider>
          <UIcon name="i-lucide-chevron-right" class="text-lg" />
        </template>
        <template #default="{ link, isActive }">
          <div
            :class="{ 'dark:text-white text-custom-800 text-lg cursor-default': isActive, 'text-custom-300 hover:text-custom-500 hover:dark:text-custom-300 dark:text-custom-500 text-lg': !isActive }"
            class="rounded-full">
            {{ link.label }}
          </div>
        </template>
      </UBreadcrumb>
    </section>
    <section class="h-4/5 w-full flex justify-center items-center">
      <div
        class="sm:w-3/4 w-full h-auto border p-5 rounded border-custom-300 bg-custom-100 dark:bg-custom-900 dark:border-custom-700">

        <UForm class="h-auto w-full flex flex-col gap-3" :state="state" @submit="onSubmit" :validate="validate"
          @error="onError">

          <header class="flex justify-between items-center">
            <div class="font-semibold cursor-default flex items-center gap-1 w-1/2">
              <UIcon name="i-lucide-user-round-plus" class="text-xl" />
              <h1 class="font-bold text-xl">New User</h1>
            </div>
          </header>


          <div class="flex justify-between">

            <h1 class="text-lg w-auto">Personal Information</h1>

            <div class="flex justify-end w-1/2 gap-x-2">

              <section class="w-auto">
                <UButton label="Cancel" icon="i-lucide-x"
                  class="flex justify-center w-full items-center rounded dark:bg-red-600 dark:hover:bg-red-500 bg-red-700 hover:bg-red-600 dark:text-custom-100"
                  to="/admin/users" />
              </section>

              <section class="w-auto">
                <UButton :label="label" :loading-icon="loadIcon" :loading="loading" icon="i-lucide-save"
                  class="flex justify-center w-full items-center rounded dark:text-white" type="submit" />
              </section>
            </div>
          </div>

          <hr class="border-custom-300 dark:border-custom-500 w-full">

          <section class="flex w-full gap-x-2">

            <!-- first name -->
            <UFormGroup class="w-1/2" label="First name" name="first_name" :ui="{ error: 'mt-1' }">

              <template #default="{ error }">
                <UInput type="text" color="gray" v-model="state.first_name" size="md" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900' } }
                }" />
              </template>

              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>

            <!-- last name -->
            <UFormGroup class="w-1/2" label="Last name" name="last_name" :ui="{ error: 'mt-1' }">

              <template #default="{ error }">
                <UInput type="text" color="gray" v-model="state.last_name" size="md" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900' } }
                }" />
              </template>

              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>

            <!-- middle initial -->
            <UFormGroup class="w-1/4" label="M. I." name="m_i">
              <template #default="{ error }">
                <UInput type="text" color="gray" size="md" v-model="state.m_i" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900 py-2' } }
                }" />
              </template>
              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>
          </section>

          <section class="flex w-full gap-x-2">

            <!-- gender -->
            <UFormGroup class="w-2/3" label="Gender" name="gender">
              <URadioGroup v-model="state.gender" :options="genderOptions" class="ml-2" />
              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>

            <!-- phone -->
            <UFormGroup class="w-full" name="phone">
              <template #label>
                <div class="flex items-center justify-start gap-1">
                  <p class="text-sm">Phone no.</p>
                  <UIcon name="i-emojione-v1-flag-for-philippines" />
                </div>
              </template>
              <template #default="{ error }">
                <UInput type="text" color="gray" size="md" v-model="state.phone" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900' } }
                }" />
              </template>
              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>
          </section>

          <section class="flex w-full gap-x-2">

            <!-- status -->
            <UFormGroup class="w-2/3" label="Status" name="status">
              <URadioGroup v-model="state.status" :options="statusOptions" class="ml-2"
                :uiRadio="{ color: state.status === statusOptions[0].value ? 'text-green-500' : 'text-red-500' }" />
            </UFormGroup>

            <!-- role -->
            <UFormGroup class="w-full" label="Role" name="role">
              <template #default="{ error }">
                <USelectMenu color="gray" selected-icon="i-heroicons-hand-thumb-up-solid" size="md" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900' } }
                }"
                  :uiMenu="{ background: 'dark:bg-custom-400', option: { color: 'dark:text-white', active: 'dark:bg-custom-600', empty: 'dark:text-white' }, empty: 'dark:text-white' }"
                  v-model="state.role" :options="roleOptions" placeholder="Select a role" />
              </template>
              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>
          </section>

          <!-- <div class="flex w-full">
          <div class="w-2/3"></div>
            <UFormGroup 
            v-if="state.role.value === roleOptions[0].value"
            label="Permission"
            name="permission" 
            help="(for some actions)" >
            <div v-for="(p) in permissionOptions" :key="p.label">
              <UCheckbox 
                v-model="state.permission[p.label.toLowerCase()]"
                class="ml-2">
                <template #label>
                  <span> {{ p.label }} </span>
                </template>
              </UCheckbox>
            </div>
            <template #error="{ error }">
              <span :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                {{ error ? error : undefined }}
              </span>
            </template>
          </UFormGroup>
        </div> -->

          <h1 class="text-lg w-auto text-start mt-3 -mb-2">Login Credentials</h1>
          <hr class="border-custom-300 dark:border-custom-500 w-full">

          <section class="flex w-full gap-x-2">

            <UFormGroup class="w-1/2" label="Username" name="username">
              <template #default="{ error }">
                <UInput type="text" color="gray" size="md" v-model="state.username" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900' } }
                }" />
              </template>
              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>

            <UFormGroup class="w-1/2" label="Password" name="password">
              <template #default="{ error }">
                <UInput type="password" color="gray" size="md" v-model="state.password" :ui="{
                  rounded: 'rounded',
                  color: error ?
                    { red: { outline: 'bg-red-100 dark:bg-red-50 text-custom-900 dark:text-custom-900 focus:ring-1 focus:ring-red-400 border-2 border-red-400 focus:border-red-400 active:ring-red-400 active:border-red-400' } } : { gray: { outline: 'dark:bg-custom-100 dark:text-custom-900' } }
                }" />
              </template>
              <template #error="{ error }">
                <span
                  :class="[error ? 'text-red-500 dark:text-red-400 text-xs font-bold' : 'text-primary-500 dark:text-primary-400']">
                  {{ error ? error : undefined }}
                </span>
              </template>
            </UFormGroup>

          </section>
        </UForm>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: 'sidebar'
})

import type { FormError, FormErrorEvent, FormSubmitEvent } from '#ui/types'

const roleOptions = [
  {
    value: 'admin',
    label: 'Admin'
  },
  {
    value: 'client',
    label: 'Client'
  }
];

// const permissionOptions = [
//   { 
//     value: false, 
//     label: 'View' 
//   },
//   { 
//     value: false, 
//     label: 'Update' 
//   },
//   { 
//     value: false, 
//     label: 'Delete' 
//   }
// ]

const statusOptions = [
  {
    value: 'active',
    label: 'Active'
  },
  {
    value: 'inactive',
    label: 'Inactive'
  }
];

const genderOptions = [
  {
    value: 'male',
    label: 'Male'
  },
  {
    value: 'female',
    label: 'Female'
  }
];

const state = reactive({
  first_name: '',
  last_name: '',
  m_i: '',
  gender: '',
  phone: '',
  status: statusOptions[0].value,
  role: '',
  // permission: {
  //   view: false,
  //   update: false,
  //   delete: false
  // },
  username: '',
  password: ''
})

const validate = (state: any): FormError[] => {
  const errors = []
  if (!state.first_name) errors.push({ path: 'first_name', message: 'Required' })
  if (!state.last_name) errors.push({ path: 'last_name', message: 'Required' })
  if (!state.gender) errors.push({ path: 'gender', message: 'Required' })
  if (!state.phone) errors.push({ path: 'phone', message: 'Required' })
  if (!state.status) errors.push({ path: 'status', message: 'Required' })

  // Check if at least one permission is selected
  // const selectedPermissions = Object.values(state.permission);
  // if (!selectedPermissions.some(permission => permission)) {
  //   errors.push({ path: 'permission', message: 'At least one permission must be selected' })
  // }

  if (!state.role) errors.push({ path: 'role', message: 'Required' })
  if (!state.username) errors.push({ path: 'username', message: 'Required' })
  if (!state.password) errors.push({ path: 'password', message: 'Required' })
  return errors
}

const loading = ref(false);
const loadIcon = ref('');
const label = ref('Save');

async function onSubmit(event: FormSubmitEvent<any>) {
  try {
    loading.value = true;
    loadIcon.value = 'i-lucide-loader-circle';
    label.value = '';

    const response = await $fetch('http://127.0.0.1:8000/api/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(state),
    });

    label.value = 'Save';
    loading.value = false;
    navigateTo('/admin/users');

    console.log('User created successfully:', response.data);
    // Optionally, you can update your UI or perform additional actions based on the response.
  } catch (error) {
    console.error('Error creating user:', error);
    // Handle error states or display error messages to the user.
  }
}

async function onError(event: FormErrorEvent) {
  const firstError = event.errors.find(error => error.path !== 'permission');
  if (firstError) {
    const element = document.getElementById(firstError.id);
    element?.focus();
    element?.scrollIntoView({ behavior: 'smooth', block: 'center' });
  }
}

const links = [{
  label: 'Users',
  to: '/admin/users'
}, {
  label: 'Create',
}]
</script>
