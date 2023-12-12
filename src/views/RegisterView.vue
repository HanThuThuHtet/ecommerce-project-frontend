<template>
    <div>
        <div class="row min-vh-100 justify-content-center mt-5">
            <div class="col-lg-4">
                <h3>Register Form</h3>
                <hr>
                <form action="" @submit.prevent="register" ref="registerForm">
                    <div class="form-floating mb-3">
                        <input type="text" class="form-control" name="name"  placeholder="John Doe">
                        <label for="floatingInput">Your Name</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input type="email" class="form-control" name="email"  placeholder="name@example.com">
                        <label for="floatingInput">Email address</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input type="password" class="form-control" name="password"  placeholder="Password">
                        <label for="floatingPassword">Password</label>
                    </div>
                    <div class="form-floating mb-3">
                        <input type="password" class="form-control" name="password_confirmation"  placeholder="Password">
                        <label for="floatingPassword">Confirm Password</label>
                    </div>
                    <div class="">
                        <button class="btn btn-primary">Register</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import {mapState,mapGetters} from "vuex";

export default{
    computed: {
        ...mapState([
            'apiUrl'
        ]),
        ...mapGetters([
            'getUrl'
        ])
    },
    methods: {
        register() {
            let formData = new FormData(this.$refs.registerForm);
            fetch(this.getUrl("/register"),{
                method : "POST",
                body : formData
            })
            .then(res => res.json())
            .then(json => {
                if(json.success === true){
                    this.$router.push('/login');
                }
            })
            ;



            //console.log(formData.get('name'),formData.get('email'));
            
        }
    },
}
</script>

<style scoped>
</style>