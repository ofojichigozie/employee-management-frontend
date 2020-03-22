<template>
    <div class="login">
        <div class="form-div">
            <form @submit.prevent="login">
                <div class="text-center">
                    <h5>ADMIN SIGN-IN</h5>
                </div>
                <div class="form-group">
                    <label for="email">Email:</label>
                    <input type="email" class="form-control" id="email" required v-model="email">
                </div>
                <div class="form-group">
                    <label for="pwd">Password:</label>
                    <input type="password" class="form-control" id="pwd" required v-model="password">
                </div>
                <input type="submit" class="form-control btn btn-dark" value="Submit"/>
            </form>
        </div>
        <div class="text-center">
            <h6>Powered by IT Team</h6>
            <i>@Employee Management System</i>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: "login",

        data(){
            return {
                email: '',
                password: ''
            }
        },

        methods: {
            login: async function(){
                if((this.email.trim().length > 0) && (this.password.trim().length > 0)){
                    try{
                        const data = {
                            email: this.email,
                            password: this.password
                        };

                        const headers = {
                            "Content-Type": "application/json"
                        };

                        const loginResponse = await axios.post('http://localhost:5000/api/admin/login', data, {headers : headers});
                        if(loginResponse.data.status == "admin_auth_success"){
                            localStorage.setItem("admin", this.email);
                            window.location.href = '/dashboard';
                        }else{
                            alert("Unauthorized access denied");
                        }

                    }catch(e){
                        await alert(e.message);
                    }
                }else{
                    await alert("Some fields are empty!");
                }
            }
        },

        beforeMount(){
            try{
                let loggedInAdmin = localStorage.getItem("admin");

                if(loggedInAdmin != null){
                    window.location.href = '/dashboard';
                }
            }catch(e){
                console.log("Error getting from local storage");
            }
        }
    }
</script>

<style scoped>
    .form-div{
        width: 35%;
        margin: 5% auto;
        padding: 20px;
        border-radius: 15px;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    }

    @media screen and (max-width: 450px){
        .form-div{
            width: 90% !important;
        }
    }
</style>