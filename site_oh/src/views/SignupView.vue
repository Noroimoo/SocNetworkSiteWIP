<template>
    <div class="max-w-7xl mx-auto grid grid-cols-2 gap-4">
        <div class="main-left">
            <div class="p-12 bg-gray-800 border border-gray-600 text-white rounded-lg">
                <h1 class="mb-6 text-2xl">Sign up</h1>

                <p class="mb-6 text-gray-300">
                    Lorem ipsum dolor sit mate. Lorem ipsum dolor sit mate. Lorem ipsum dolor sit mate.
                    Lorem ipsum dolor sit mate. Lorem ipsum dolor sit mate. Lorem ipsum dolor sit mate.
                </p>

                <p class="font-bold text-gray-200">
                    Already have an account? <RouterLink :to="{'name': 'login'}" class="underline text-purple-300">Click here</RouterLink> to log in!
                </p>
            </div>
        </div>

        <div class="main-right">
            <div class="p-12 bg-gray-800 border border-gray-600 text-white rounded-lg">
                <form class="space-y-6" v-on:submit.prevent="submitForm">
                    <div>
                        <label class="text-gray-200">Name</label><br>
                        <input type="text" v-model="form.name" placeholder="Your full name" class="w-full mt-2 py-4 px-6 border border-gray-600 text-white bg-gray-900 rounded-lg">
                    </div>

                    <div>
                        <label class="text-gray-200">E-mail</label><br>
                        <input type="email" v-model="form.email" placeholder="Your e-mail address" class="w-full mt-2 py-4 px-6 border border-gray-600 text-white bg-gray-900 rounded-lg">
                    </div>

                    <div>
                        <label class="text-gray-200">Password</label><br>
                        <input type="password" v-model="form.password1" placeholder="Your password" class="w-full mt-2 py-4 px-6 border border-gray-600 text-white bg-gray-900 rounded-lg">
                    </div>

                    <div>
                        <label class="text-gray-200">Repeat password</label><br>
                        <input type="password" v-model="form.password2" placeholder="Repeat your password" class="w-full mt-2 py-4 px-6 border border-gray-600 text-white bg-gray-900 rounded-lg">
                    </div>

                    <template v-if="errors.length > 0">
                        <div class="bg-red-500 text-white rounded-lg p-6">
                            <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                        </div>
                    </template>

                    <div>
                        <button class="py-4 px-6 bg-purple-700 text-white rounded-lg">Sign up</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

import { useToastStore } from '@/stores/toast'


export default {
    setup() {
        const toastStore = useToastStore()

        return {
            toastStore
        }
    },

    data() {
        return {
            form: {
                email: '',
                name: '',
                password1: '',
                password2: ''
            },
            errors: [],
        }
    },

    methods: {
        submitForm() {
            this.errors = []

            if (this.form.email === '') {
                this.errors.push('Your e-mail is missing')
            }

            if (this.form.name === '') {
                this.errors.push('Your name is missing')
            }

            if (this.form.password1 === '') {
                this.errors.push('Your password is missing')
            }

            if (this.form.password1 !== this.form.password2) {
                this.errors.push('The password does not match')
            }

            if (this.errors.length === 0) {
                axios
                    .post('/api/signup/', this.form)
                    .then(response => {
                        if (response.data.message === 'success') {
                            this.toastStore.showToast(5000, 'The user is registered. Please log in', 'bg-emerald-500')

                            this.form.email = ''
                            this.form.name = ''
                            this.form.password1 = ''
                            this.form.password2 = ''
                        } else {
                            this.toastStore.showToast(5000, 'Something went wrong. Please try again', 'bg-red-300')
                        }
                    })
                    .catch(error => {
                        console.log('error', error)
                    })
            }
        }
    }
}
</script>