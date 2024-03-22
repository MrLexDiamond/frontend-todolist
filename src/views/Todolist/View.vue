<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h4>
          Todo List Table
          <RouterLink to="/todolist/create" class="btn btn-primary float-end">
            Add List
          </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>ID</th>
              <th>Tasks</th>
              <th>Status</th>
              <th>Date Create</th>
              <th>Date Updated</th>
              <th>Action Update</th>
              <th>Action Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(todolist, index) in this.todolist" :key="index">
              <td>{{ todolist.id }}</td>
              <td>{{ todolist.tasks }}</td>
              <td>{{ todolist.status }}</td>
              <td>{{ todolist.created_at }}</td>
              <td>{{ todolist.updated_at }}</td>
              <td>
                <RouterLink :to="{ path: '/todolist/'+todolist.id+'/edit'}" class="btn btn-success">
                  Update
                </RouterLink>
              </td>
              <td>
                <button type="button" @click="deleteTodolist(todolist.id)" class="btn btn-danger">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'todolist',
  data(){
    return {
      todolist: []
    }
  },
  mounted(){
    this.getTodolist();
    //console.log('am here')
  },
  methods: {
    getTodolist(){
      axios.get('http://localhost:8000/api/todolist').then(res => {
        this.todolist = res.data.todolist
        console.log(this.todolist)
      });
    },

    deleteTodolist(todolistId){
      if(confirm('Are you sure, your want to delete this data?')){
        //console.log(todolistId)
        axios.delete(`http://localhost:8000/api/todolist/${todolistId}/delete`)
        .then(res => {
          alert(res.data.message);
          this.getTodolist();
        })
        .catch(function (error) {
            if(error.response) {

                if(error.response.status == 404) {
                    alert(error.response.data.message);
                }
            }
        });
      }
    },
  },
}

</script>
