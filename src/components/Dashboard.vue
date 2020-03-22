<template>
    <div class="dashboard">
        <div class="header">
            <h6><span style="color: #FFFF00; font-style: italic;">Employee</span> Management System</h6>
            <a href="#" class="btn btn-danger" v-on:click.prevent="logout">Sign out</a>
        </div>
        <div class="landing">

        </div>
        <div class="row">
            <div class="col-md-7 col-sm-12">
                <form @submit.prevent="createEmployee">
                    <h5>
                        CREATE ACCOUNT FOR A NEW EMPLOYEE
                    </h5>
                    <div class="form-group">
                        <label for="surname">Surname:</label>
                        <input type="text" class="form-control" id="surname" required v-model="surname">
                    </div>
                    <div class="form-group">
                        <label for="otherNames">Other Names:</label>
                        <input type="text" class="form-control" id="otherNames" required v-model="otherNames">
                    </div>
                    <div class="form-group">
                        <label for="sex">Sex:</label>
                        <select class="form-control" id="sex" required v-model="sex">
                            <option value="Male">Male</option>
                            <option value="Female">Female</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="maritalStatus">Marital Status:</label>
                        <select class="form-control" id="maritalStatus" required v-model="maritalStatus">
                            <option value="Single">Single</option>
                            <option value="Married">Married</option>
                            <option value="Divorced">Divorced</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="email">Email:</label>
                        <input type="email" class="form-control" id="email" required v-model="email">
                    </div>
                    <div class="form-group">
                        <label for="contact">Contact:</label>
                        <input type="tel" class="form-control" id="contact" required v-model="contact">
                    </div>
                    <div class="form-group">
                        <label for="UUID">UUID:</label>
                        <input type="text" class="form-control" id="UUID" required v-model="UUID">
                    </div>
                    <div class="form-group">
                        <label for="position">Position:</label>
                        <input type="text" class="form-control" id="position" required v-model="position">
                    </div>
                    <input type="submit" class="form-control btn btn-dark" value="Create account"/>
                </form>
            </div>
            <div class="col-md-1 col-sm-12">
                <p></p>
            </div>
            <div class="col-md-4 col-sm-12">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title" style="color: #FFFF00;">VERIFICATION</h5>
                        <p class="card-text">Enter an employee's UUID for verification</p>
                        <form @submit.prevent="verifyEmployee()">
                            <div class="form-group">
                                <!-- <label for="email">Email:</label> -->
                                <input type="text" class="form-control" id="email" placeholder="UUID" required v-model="vUUID">
                            </div>
                            <div class="form-check">
                                <label class="form-check-label">
                                <input class="form-check-input" type="checkbox" v-model="viewEmployeeDetailsCheck"> View employee's details
                                </label>
                            </div>
                            <p></p>
                            <input type="submit" class="form-control btn btn-danger" value="Check"/>
                        </form>
                    </div>
                </div>
                <div v-if="verificationFinished == true">
                    <div v-if="verifyWithDetailsShown == true">
                        <h6 class="text-center mt-2">{{ verificationSuccessMessage }}</h6>
                        <div class="card">
                            <div class="card-body">
                                <h6 class="card-title text-center" style="color: #FFFF00;">Employee Details</h6>
                                <table class="table table-light">
                                    <tbody>
                                        <tr>
                                            <td>Surname: </td>
                                            <td>{{ verifiedEmployee.surname }}</td>
                                        </tr>
                                        <tr>
                                            <td>Other Names: </td>
                                            <td>{{ verifiedEmployee.otherNames }}</td>
                                        </tr>
                                        <tr>
                                            <td>Sex: </td>
                                            <td>{{ verifiedEmployee.sex }}</td>
                                        </tr>
                                        <tr>
                                            <td>Marital Status: </td>
                                            <td>{{ verifiedEmployee.maritalStatus }}</td>
                                        </tr>
                                        <tr>
                                            <td>Email: </td>
                                            <td>{{ verifiedEmployee.email }}</td>
                                        </tr>
                                        <tr>
                                            <td>Position: </td>
                                            <td>{{ verifiedEmployee.position }}</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                    <div v-else-if="verifyWithDetailsShown == false">
                        <h4>{{ verificationSuccessMessage }}</h4>
                    </div>
                </div>
            </div>
        </div>
        <div class="all-employees">
            <div class="text-center">
                <button class="btn btn-danger mb-3" @click="readEmployees">See all employees</button>
            </div>
            <div v-if="hasRetrievedEmployees == true">
                <div style="overflow-x: auto;">
                    <table class="table table-bordered table-stripped">
                        <thead class="thead-dark">
                            <th>Surname</th>
                            <th>Other Names</th>
                            <th>Sex</th>
                            <th>Marital Status</th>
                            <th>Email</th>
                            <th>Contact</th>
                            <th>UUID</th>
                            <th>Position</th>
                            <th></th>
                            <th></th>
                        </thead>
                        <tbody>
                            <tr v-for="employee in allEmployees" v-bind:key="employee.id">
                                <td>{{ employee.surname }}</td>
                                <td>{{ employee.otherNames }}</td>
                                <td>{{ employee.sex }}</td>
                                <td>{{ employee.maritalStatus }}</td>
                                <td>{{ employee.email }}</td>
                                <td>{{ employee.contact }}</td>
                                <td>{{ employee.uuid }}</td>
                                <td>{{ employee.position }}</td>
                                <td><i class="fa fa-edit" style="color: #AA0000" v-on:click.prevent="showUpdateModal(employee)"></i></td>
                                <td><i class="fa fa-trash"  style="color: #AA0000" v-on:click.prevent="deleteEmployee(employee._id)"></i></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal Dialog -->
        <UpdateModal class="modal fade" id="updateModal" v-bind:employeeOriginalData="updateModalEmployeeData"/>
    </div>
</template>

<script>
    import axios from 'axios';
    import UpdateModal from './UpdateModal';

    export default {
        name: "dashboard",

        components: {
            UpdateModal
        },

        data(){
            return {
                //Registration details
                surname: "",
                otherNames: "",
                sex: "",
                maritalStatus: "",
                email: "",
                contact: "",
                UUID: "",
                position: "",

                //Verification detail
                vUUID: "",
                verifiedEmployee: {},
                viewEmployeeDetailsCheck: false,
                verifyWithDetailsShown: false,
                verificationFinished: false,
                verificationSuccessMessage: "",

                hasRetrievedEmployees: false,
                allEmployees: [],

                updateModalEmployeeData: {}
            }
        },

        methods: {
            logout: function(){
                try{
                    localStorage.removeItem("admin");
                }catch(e){
                    console.log("Error removing from local storage");
                }
                
                window.location.href = '/';
            },

            createEmployee: async function(){
                if((this.surname.trim().length > 0) && (this.otherNames.trim().length > 0) && (this.UUID.trim().length > 0) && (this.position.trim().length > 0)){
                    //Data to post to server
                    const data = {
                        surname: this.surname,
                        otherNames: this.otherNames,
                        sex: this.sex,
                        maritalStatus: this.maritalStatus,
                        email: this.email,
                        contact: this.contact,
                        UUID: this.UUID,
                        position: this.position
                    };

                    //Setting headers
                    const headers = {
                        "Content-Type": "application/json"
                    };

                    //Make post request to API endpoint to store data
                    try{
                        const registerResponse = await axios.post("http://localhost:5000/api/employee/register", data, {headers: headers});
                        if(registerResponse.data.status == "employee_reg_success"){
                            //Clear the inputs fields
                            this.surname = this.otherNames = this.email = this.contact = this.UUID = this.position = "";

                            alert("Employee registration was successful!");
                        }else{
                            alert("Could not register the new employee");
                        }
                    }catch(e){
                        alert(e.message);
                    }
                }else{
                    alert("Some fileds are empty!");
                }
            },

            verifyEmployee: async function(){
                if(this.vUUID.trim().length > 0){
                    try{
                        const verificationResponse = await axios.get("http://localhost:5000/api/employee/verification/"+this.vUUID);
                        if(verificationResponse.data.status == "verification_succeeded"){
                            //Set this variable to true to show that verification has finished
                            this.verificationFinished = true;
                            this.verificationSuccessMessage = `Employee with UUID, ${this.vUUID} was verified successfully!`;

                            //Check if admin wish to view employee details
                            if(this.viewEmployeeDetailsCheck){
                                this.verifyWithDetailsShown = true;
                                this.verifiedEmployee = verificationResponse.data.employee;
                            }

                        }else{
                            alert("Could not verify the employee");
                        }
                    }catch(e){
                        console.log(e.message);
                    }
                }else{
                    await alert("Enter employee UUID for verification");
                }
            },

            readEmployees: async function(){
                try{
                    const readEmployeesResponse = await axios.get("http://localhost:5000/api/employee/employees");
                    if(readEmployeesResponse.data.status == "employees_success"){
                        this.allEmployees = readEmployeesResponse.data.data;
                        this.hasRetrievedEmployees = true;
                    }else{
                        alert("Could not retrieve employees");
                    }
                }catch(e){
                    console.log(e.message);
                }
            },

            deleteEmployee: async function(param){
                let confirmRes = confirm("This employee will be deleted permanently. Continue?");

                if(confirmRes == true){
                    try{
                        let deleteResponse = await axios.delete("http://localhost:5000/api/employee/delete/"+param);
                        if(deleteResponse.data.status == "employee_deleted"){
                            alert("The employee was deleted");
                        }else{
                            alert("Could not delete employee");
                        }
                    }catch(e){
                        console.log(e.message);
                    }
                }
            },

            showUpdateModal: function(param){
                this.updateModalEmployeeData = param;
                $('#updateModal').modal('show');
            }
        }
    }
</script>

<style scoped>
    .header{
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0px 20px;
        background: #000000;
        color: #FFFFFF;
        height: 85px;
        border-bottom: 1px solid gray;
    }

    .landing{
        background: lightgray;
        height: 400px;
        background: linear-gradient(180deg, rgba(0, 0, 0, 0.75) 25.62%, rgba(0, 0, 0, 0.75) 100%), url('~@/assets/original-back.jpg');
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover
    }

    .row{
        width: 90%;
        margin: 25px auto;
    }

    .card{
        background: #000000;
        color: #FFFFFF;
    }

    .all-employees{
        width: 90%;
        margin: 10px auto;
    }
</style>