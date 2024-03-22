<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Update Todo List</h4>
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
                    <button type="button" @click="updateTodolist" class="btn btn-primary">Update</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'todolistEdit',
    data(){
        return{
            todolistId: '',
            errorList: '',
            model: {
                todolist: {
                    tasks: '',
                    status: false
                }
            }
        }
    },
    mounted(){
        //console.log(this.$route.params.id);
        this.todolistId = this.$route.params.id;
        this.getTodolistData(this.$route.params.id);
    },  
    methods: {
        getTodolistData(todolistId){
            axios.get(`http://localhost:8000/api/todolist/${todolistId}/edit`)
            .then(res => {
                console.log(res.data.todolist);

                this.model.todolist = res.data.todolist
            })
            .catch(function (error) {
                if(error.response) {

                    if(error.response.status == 404) {
                        alert(error.response.data.message);
                    }
                }
            });
        },
        updateTodolist(){
            var mythis = this;
            if(confirm('Are you sure, your want to Update this data?')){
                axios.put(`http://localhost:8000/api/todolist/${this.todolistId}/edit`, this.model.todolist)
            .then(res => {
                console.log(res.data)
                alert(res.data.message)

                this.errorList = '';
            })
            .catch(function (error) {
                if(error.response) {

                    if(error.response.status == 422) {
                        mythis.errorList = error.response.data.errors;
                    }
                    if(error.response.status == 404) {
                        alert(error.response.data.message);
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