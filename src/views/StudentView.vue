<template>
    <div>
        <h1>This is Student Page</h1>
        <input type="button" value="Add Student" @click="isAdd = !isAdd">
        <table v-if="students != null">
            <tr>
                <td>#</td>
                <td>Name</td>
                <td>Age</td>
                <td>Gender</td>
            </tr>
            <tr v-for="(value, key) in students" v-bind:key="key">
                <td>{{ value.id }}</td>
                <td>{{ value.name }}</td>
                <td>{{ value.age }}</td>
                <td>{{ value.gender }}</td>
                <td><input type="button" value="Edit" @click="editStudent(value)"></td>
                <td><input type="button" value="Delete" @click="deleteStudent(value.id)"></td>
            </tr>
        </table>
        <div v-if="isAdd">
            <h1>Add Student</h1>
            <input type="text" placeholder="Name" v-model="student.name" /> <br>
            <input type="number" placeholder="age" v-model="student.age" /> <br>
            <select v-model="student.gender">
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="others">Others</option>
            </select> <br>
            <input type="button" value="Submit" @click="addStudent">
        </div>
        <div v-if="isEdit">
            <h1>Edit Student</h1>
            <input type="text" placeholder="Name" v-model="student.name" /> <br>
            <input type="number" placeholder="age" v-model="student.age" /> <br>
            <select v-model="student.gender">
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="others">Others</option>
            </select> <br>
            <input type="button" value="Submit" @click="updateStudent">
        </div>        
    </div>
</template>
<script>
export default {
    data(){
        return {
            students: null,
            isAdd: false,
            isEdit: false,
            student: {
                id: 0,
                name: "",
                age: 0,
                gender: ""
            }
        }
    },
    mounted(){
      this.getStudents();
    },
    methods:{
        getStudents: function () {
            this.axios.get("/student/get/all").then((response) => {
                this.students = response.data;
                console.log(response.data)
            })
        },
        addStudent: function () {
            this.axios.post("/student/add", this.student).then((response) => {                
                console.log(response.data)
                this.isAdd = false;
                this.getStudents();
            })            
        },
        editStudent: function (student) {
            this.isEdit = ! this.isEdit;
            this.student = student;
            console.log(student);
        },
        updateStudent: function () {
            this.axios.post("/student/update", this.student).then((response) => {                
                console.log(response.data)
                this.isEdit = false;
                this.getStudents();
            })            
        },        
        deleteStudent: function (id) {
            this.axios.get("/student/delete/"+id).then((response) => {                
                console.log(response.data)
                this.getStudents();
            })
        },        
        
    }

}
</script>