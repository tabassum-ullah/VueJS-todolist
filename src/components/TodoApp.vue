<template>
  <div class="container">
    <h2 class="text-center mt-5">To-do App</h2>
  </div>
  <div class="d-flex">
    <input v-model="task" type="text" placeholder="Enter text" class="form-control">
    <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
  </div>
  <table class="table table-bordered mt-5">
  <thead>
    <tr class="heading">
      <th scope="col" class="text-center">Task</th>
      <th scope="col" class="text-center">Status</th>
      <th scope="col" class="text-center">Edit</th>
      <th scope="col" class="text-center">Delete</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task,index) in tasks" :key="index">
      <td class="taskbody">
        <span :class="{ 'finished': task.status === 'finished' }">
          {{task.name}}
        </span>
      </td>
      <td class="statusbody">
        <span @click="changeStatus(index)" class="pointer"
          :class="{'text-danger': task.status==='to-do', 'text-warning': task.status==='in-progress', 'text-success': task.status==='finished'}"
        >
          {{ firstcharUpper(task.status) }}
        </span>
      </td>
      <td>
        <div class="text-center pointer" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center pointer" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>    
  </tbody>
</table>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data(){
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished'],
      tasks: [
        {
          name: 'First to-do item.',
          status: 'finished'
        },
        {
          name: 'Second to-do item.',
          status: 'in-progress'
        },
        {
          name: 'Third to-do item.',
          status: 'to-do'
        }
      ]
    }
  },
  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task='';
    },
    deleteTask(index){
      this.tasks.splice(index,1);
    },
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstcharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>
<style scoped src="@/assets/styles/TodoApp.css">
</style>
