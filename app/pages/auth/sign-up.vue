<template>
    <div class="flex flex-col items-center justify-center h-screen">
        <div class="w-full max-w-md p-6">
            <h1 class="font-anton text-center mb-6 text-5xl">
                {{ type === 'sign-up' ? 'Sign Up' : 'Sign In' }}
            </h1>
            <UForm :state="formData" @submit="onSubmit" class="space-y-4">
                <UFormField label="Email" name="email">
                    <UInput 
                        v-model="formData.email" 
                        type="email" 
                        placeholder="Email" 
                        class="w-full"
                    />
                </UFormField>
                <UFormField label="Password" name="password">
                    <UInput 
                        v-model="formData.password" 
                        type="password" 
                        placeholder="Password" 
                        class="w-full"
                    />
                </UFormField>

                <UButton 
                    type="button" 
                    trailing-icon="i-lucide-arrow-left"
                    variant="text"
                    color="gray"
                    @click="navigateTo('/')"
                     
                    class="w-full" block
                >
                    Go Back
                </UButton>
                <UButton 
                    type="submit" 
                    trailing-icon="i-lucide-arrow-right"
                    color="primary"
                    :loading="loading" 
                    class="w-full" block
                >
                    {{ type === 'sign-up' ? 'Sign Up' : 'Sign In' }}
                </UButton>
                <USeparator /> 
                <UButton 
                    trailing-icon="mdi:account-alert-outline"
                    variant="ghost"
                    color="info"
                    @click="type = type === 'sign-up' ? 'sign-in' : 'sign-up'"
                     
                    class="w-full" block
                >
                    {{ type === 'sign-up' ? 'Already have an account? Sign In' : 'Don\'t have an account? Sign Up' }}
                </UButton>
            </UForm>

        </div>
    </div>
</template>

<script setup>
    import * as yup from 'yup';

    const type = ref('sign-up');
    const loading = ref(false);
    const formData = reactive({
        email: 'rogers@microskills.ac.tz',
        password: '3xampl3p4ssw0rd',
    });

    async function onSubmit(event) {
        event.preventDefault();
        console.log(formData);
    }

    definePageMeta({
        layout: 'auth'
    })
</script>