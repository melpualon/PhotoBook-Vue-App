<template>
    <div>
        <form v-if="!confirmPassword" class="flex flex-col items-center" @submit.prevent="signUp">
            <div class="flex flex-col user">
                <label for="username" class="block text-gray-700 text-sm font-bold mb-2">Username</label>
                <input type="text" class="shadow appearance-none border rounded w-full py-2 px-3" v-model="username" id="username">
            </div>
            <div class="flex flex-col mt-2">
                <label for="password" class="block text-gray-700 text-sm font-bold mb-2">Password</label>
                <input type="password" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700" v-model="password" id="password">
            </div>
            <div class="flex flex-col mt-2">
                <label for="email" class="block text-gray-700 text-sm font-bold mb-2">Email</label>
                <input type="email" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700" v-model="email" id="email">
            </div>
            <button class="btn-blue mt-10">Sign Up</button>
        </form>
        <div v-if="error" class="text-red-500">
            {{ error.message }}
        </div>
        <div v-if="confirmPassword" class="w-4/12 m-auto">
            <h3>Enter your code. Please check your email</h3>
            <div class="flex flex-col mt-2">
                <label for="password" class="block text-gray-700 text-sm font-bold mb-2">Code</label>
                <input type="text" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700" v-model="code">
                <button class="btn-blue " @click="confirmSignUp">Confirm Code</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        username: '',
        password: '',
        email: '',
        error: '',
        confirmPassword: false,
        code: ''
    }),
    methods: {
        async signUp() {
            if(!this.email || !this.password) {
                return;
            }
            try {
                await this.$store.dispatch('auth/signup', {
                    username: this.username,
                    password: this.password,
                    emai: this.email
                })

                this.confirmPassword = true;
            } catch(error) {
                this.error = error;
            }
        },
        async confirmSignUp() {
            if(!this.username || !this.code) {
                return
            }

            try {
                await this.$store.dispatch('auth/confirmSignUp', {
                    username: this.username,
                    code: this.code
                })

                await this.$store.dispatch('authlogin', {
                    username: this.username,
                    password: this.password
                })
                this.$router.push('/albums')
            } catch (error) {
                console.log(error)
                this.error = error
            }
        }
    }
}
</script>

<style>

</style>