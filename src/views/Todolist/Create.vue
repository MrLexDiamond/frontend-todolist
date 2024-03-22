<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Add Todo List</h4>
            </div>
            <div class="card-body">
                <ul class="alert alert-warning" v-if="Object.keys(this.errorList).lenght > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="Tasks">Task Name</label>
                    <input type="text" v-model="model.todolist.tasks" class="form-control"/>
                </div>
                <div class="mb-3">
                    <button type="button" @click="saveTodolist" class="btn btn-primary">Save</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'todolistCreate',
    data(){
        return{
            errorList: '',
            model: {
                todolist: {
                    tasks: '',
                    status: false
                }
            }
        }
    },
    methods: {
        saveTodolist(){
            var mythis = this;
            if(confirm('Are you sure, your want to Add this data?')){
                axios.post('http://localhost:8000/api/todolist', this.model.todolist)
            .then(res => {
                console.log(res.data)
                alert(res.data.message)

                this.model.todolist = {
                    tasks: '',
                    status: 'pending'
                }
                this.errorList = '';
            })
            .catch(function (error) {
                if(error.response) {

                    if(error.response.status == 422) {
                        mythis.errorList = error.response.data.errors;
                    }
                    //console.log(error.response.data);
                    //console.log(error.response.status);
                    //console.log(error.response.headers);
                } else if (error.request) {
                    console.log(error.request);
                } else {
                   console.log('Error', error.message);
                }
            });
            }
        }
    },
}
</script>