<template>
    <div>
        <div class="row">
            <div class="col-sm-8 offset-2">
                <h1>My Register Page</h1>
                <div v-if="success" class="alert-success">
                    {{ success }}
                </div>
                <form @submit.prevent="register">
                    <div class="form-group">
                        <input v-model="userName" placeholder="username" class="form-control">
                        <span v-if="errorUserName" class="error">{{ errorUserName }}</span>
                    </div>
                    <div class="form-group">
                        <input v-model="email" placeholder="email" class="form-control">
                        <span v-if="errorEmail" class="error">{{ errorEmail }}</span>
                    </div>
                    <div class="form-group">
                        <input v-model="password" placeholder="password" class="form-control" type="password">
                        <span v-if="errorPassword" class="error">{{ errorPassword }}</span>
                    </div>
                    <div class="form-group">
                        <input v-model="password_confirmation" placeholder="confirm password" type="password" class="form-control">
                        <span v-if="errorPassword_confirmation" class="error">{{ errorPassword_confirmation }}</span>
                    </div>

                    <input type="submit" value="register">
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Register",
    data() {
        return {
            userName: "",
            email: "",
            password: "",
            password_confirmation: "",
            success: "",
            errorUserName: "",
            errorEmail: "",
            errorPassword: "",
            errorPassword_confirmation: ""
        };
    },
    methods: {
        async register() {
            this.$axios.post('register',
                {
                    name: this.userName,
                    email: this.email,
                    password: this.password,
                    password_confirmation: this.password_confirmation
                })
                .then(response => {
                    this.success = response.data.success;
                    this.deleteUserData();
                    this.$router.push('/login');
                    console.log(response,'registered')
                })
                .catch(error => {
                        this.errorUserName = "",
                        this.errorEmail = "",
                        this.errorPassword = "",
                        this.errorPassword_confirmation = ""
                    if (error.response.data.errors) {
                        if (error.response.data.errors.name) {
                            this.errorUserName = error.response.data.errors.name[0];
                        }
                        if (error.response.data.errors.email) {
                            this.errorEmail = error.response.data.errors.email[0];
                        }
                        if (error.response.data.errors.password) {
                            this.errorPassword = error.response.data.errors.password[0];
                        }
                        if (error.response.data.errors.password_confirmation) {
                            this.errorPassword_confirmation = error.response.data.errors.password_confirmation[0];
                        }
                    }
                });
        },
        deleteUserData() {
            this.userName = "",
            this.email = "",
            this.password = "",
            this.password_confirmation = ""
        }
    }
}
</script>


<style scoped>
.error {
    color:red;
}
</style>
