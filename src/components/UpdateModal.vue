<template>
    <!-- <div id="update-modal"> -->
        <!-- The Modal -->
        <div class="modal fade" id="updateModal">
            <div class="modal-dialog">
                <div class="modal-content">

                <!-- Modal Header -->
                <div class="modal-header">
                    <h6 class="modal-title">EDIT EMPLOYEE ACCOUNT</h6>
                    <button type="button" class="close" data-dismiss="modal">&times;</button>
                </div>

                <!-- Modal body -->
                <div class="modal-body">
                    <form @submit.prevent="updateEmployee">
                        <div class="form-group">
                            <label for="surname">Surname:</label>
                            <input type="text" class="form-control" id="surname" name="surname" required v-model="employeeOriginalData.surname">
                        </div>
                        <div class="form-group">
                            <label for="otherNames">Other Names:</label>
                            <input type="text" class="form-control" id="otherNames" name="otherNames" required v-model="employeeOriginalData.otherNames">
                        </div>
                        <div class="form-group">
                            <label for="sex">Sex:</label>
                            <select class="form-control" id="sex" name="sex" required v-model="employeeOriginalData.sex">
                                <option value="Male">Male</option>
                                <option value="Female">Female</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="maritalStatus">Marital Status:</label>
                            <select class="form-control" id="maritalStatus" name="maritalStatus" required v-model="employeeOriginalData.maritalStatus">
                                <option value="Single">Single</option>
                                <option value="Married">Married</option>
                                <option value="Divorced">Divorced</option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="email">Email:</label>
                            <input type="email" class="form-control" id="email" name="email" required v-model="employeeOriginalData.email">
                        </div>
                        <div class="form-group">
                            <label for="contact">Contact:</label>
                            <input type="tel" class="form-control" id="contact" name="contact" required v-model="employeeOriginalData.contact">
                        </div>
                        <div class="form-group">
                            <label for="UUID">UUID:</label>
                            <input type="text" class="form-control" id="UUID" name="UUID" required readonly v-model="employeeOriginalData.uuid">
                        </div>
                        <div class="form-group">
                            <label for="position">Position:</label>
                            <input type="text" class="form-control" id="position" name="position" required v-model="employeeOriginalData.position">
                        </div>
                        <input type="submit" class="form-control btn btn-dark" value="Update account"/>
                    </form>
                </div>

                </div>
            </div>
        </div>
    <!-- </div> -->
</template>

<script>
    import axios from 'axios';

    export default {
        name: "update-modal",

        data(){
            return {
                
            }
        },

        props: [
            'employeeOriginalData'
        ],

        methods: {
            updateEmployee: async function(e){
                const data = {
                    surname: e.target.elements.surname.value,
                    otherNames: e.target.elements.otherNames.value,
                    sex: e.target.elements.sex.value,
                    maritalStatus: e.target.elements.maritalStatus.value,
                    email: e.target.elements.email.value,
                    contact: e.target.elements.contact.value,
                    UUID: e.target.elements.UUID.value,
                    position: e.target.elements.position.value
                }

                //Setting headers
                const headers = {
                    "Content-Type": "application/json"
                };

                //Make post request to API endpoint to store data
                try{
                    const updateResponse = await axios.patch("http://localhost:5000/api/employee/update", data, {headers: headers});
                    if(updateResponse.data.status == "employee_updated"){
                        alert("Updated employee record successfully!");
                    }else{
                        alert("Could not update employee records!");
                    }
                }catch(e){
                    await alert(e.message);
                }
            }
        }
    }
</script>

<style scoped>

</style>