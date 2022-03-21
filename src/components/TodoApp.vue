<script setup>
import { ref } from 'vue'
import draggable from 'vuedraggable'
import Navbar from './Navbar.vue'


</script>

<template >
<Navbar></Navbar>
 <!-- input -->
 <div id="todo-wrapper" class="grid bg-purple-500 p-2 rounded-lg">
   <header class="header">
 <input v-model="task" class="input mb-2 border-2 p-1 rounded-lg pl-4" type="text" placeholder="enter your todo"    @keyup.enter="submitTask">
  <button class="new-todo-button"
        @click="submitTask"  
        v-show="task.length > 0"
      ></button>
   </header>

      <span id="set_done_wrapper" class="flex mb-5">
 <button @click="setAllDone" class="all_complete">Set All Done</button>
 <button @click="deleteAllDone" class="delete">Delete All Done</button>
      </span>



 <table class="mb-10">
  <thead>
    <tr>
      <th class="border-none">Task</th>
      <th class="border-none">Status</th>
      <th class="border-none">Edit</th>
      <th class="border-none">Delete</th>
    </tr>
  </thead>
  <tbody>
    <!--@note Filtered tasks is a computed value based on the visibility  -->
  

    <tr v-for="(task, index) in filteredTasks" :key="index">
      <td class="border border-slate-300 ..."><span :class="{'done': task.status ==='done'}">{{task.name}}</span></td>
      <td class="border border-slate-300 ..." @click="changeStatus(index)" :class="{'text-sky-600': task.status ==='not started' ,
      'text-yellow-400': task.status ==='doing',
      'text-green-500': task.status ==='done',
      
      }" :style="{width: 120+'px'}"><span class="cursor-pointer capitalize">{{task.status}}</span></td>
      <td class="border border-slate-300 ...">
           <div class="text-center" @click="editTask(index)">
        <svg xmlns="http://www.w3.org/2000/svg"  fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
     <path stroke-linecap="round" stroke-linejoin="round" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
      </svg>
      </div>
      </td>

            <td class="border border-slate-300 ...">
              <div class="text-center" @click="deleteTask(index)">
              <svg xmlns="http://www.w3.org/2000/svg"  fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
        <path stroke-linecap="round" stroke-linejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
      </svg>
      </div>
      </td>
    </tr>

  </tbody>
</table>

<span class="filterbar bg-opacity-20 p-1">
<span class="text-white">{{remaining}} Tasks left</span>

<span class="filterbuttons">
  <button @click="filterAll" class="border-none text-white filterbutton">All </button>
 <button @click="filterActive" class="border-none text-white filterbutton">Active Tasks</button>
  <button @click="filterDone" class="border-none text-white filterbutton" >Done</button>
</span>
</span>
 </div>
</template>
<script>



// visibility filters
var filters = {
  all: function(todos) {
    return todos;
  },
  active: function(tasks) {
    return tasks.filter(function(task) {
      if (task.status !== 'done') {
        return task
      }
    });
  },
  completed: function(tasks) {
    return tasks.filter(function(task) {
    if (task.status === 'done') {
    return task;  
    }
    });
  }
};

export default {
  name: "HelloWorld",
  
    computed: {
    filteredTasks: function() {
      return filters[this.visibility](this.tasks);
    },
    remaining: function() {
      return filters.active(this.tasks).length;
    },
  },
  
  data(){
    return {
      task: '',
      editedTask: null,
      visibility: "all",
      availableStatuses: ['not started', 'doing' ,'done'],
      tasks: JSON.parse(localStorage.getItem(('tasks'))) || []
    }
  },
  methods: {
   
    submitTask(){
      console.log(this.task)
      if (this.task.length===0) return;

    if (this.editedTask === null) {
      this.tasks.push({
        name: this.task,
        status: "pending"
      })
    } else {
      this.tasks[this.editedTask].name = this.task;
      this.editedTask = null;
    }
      this.saveToStorage();
      this.task = '';
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
      this.saveToStorage();


    },
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask=index;
      this.saveToStorage();
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
      this.saveToStorage();
    },

    setAllDone(){
      this.tasks.forEach(task => task.status='done');
      this.saveToStorage();
    },

    deleteAllDone() {
      this.tasks = filters.active(this.tasks)
      console.log(this.tasks) 
      this.saveToStorage();
    },

    getRemaining(){
       this.tasks.forEach(task => task.status !== 'done')
    },

    filterDone() {
      this.visibility = "completed"
    },

    filterActive() {
      this.visibility = "active"
    },

    filterAll() {
      this.visibility = "all"
    },
    
     saveToStorage() {
       console.log(this.tasks)
       localStorage.setItem("tasks", JSON.stringify(this.tasks))
    },
  }
  
}
</script>
<style scoped>
  header {
    display: grid;
    grid-template-columns: 300px auto;
  }

  #todo-wrapper {
    display:block;
    max-width: 23rem;
  }

  td { 
  border:none;
  background-color: rgba(0, 0, 0, 0.2);
  }

  table { 
    border-spacing: 20px;
  }



  #set_done_wrapper {
    justify-content: space-between;
  }

  #set_done_wrapper button{
    color: #FEC260;
  }
  #set_done_wrapper button:hover{
  color: #f3a424
  }



  input { 
    background-color: #00000020;
    color:white;
    border:none;
    height: 62px;
  }

  input::placeholder {
    color:rgba(255, 255, 255, 0.20);
  }

  .filterbar {
    background-color: #fec16020;
  }

  table { 
    width: 350px; 
    border-collapse: separate;
    border-spacing: 0 15px;
  }
  .text-white {
    font-size:12px;
  }

  .filterbuttons button{
    background-color:#ffffff7a;
    text-align: center;
    padding:0px 5px 0px 5px;
  }
  .filterbuttons button:hover{
    background-color:#ff00007a;
    text-align: center;
    padding:0px 5px 0px 5px;
  }

  tr { 
  border-spacing:5em;
    border-radius:10px;
  }

  tr td:first-child {
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
  }

  tr td:nth-child(2):hover {
  background-color:rgba(97, 255, 57, 0.336);

  }

  tr td:last-child {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
  }


  td {
    padding: 20px;
    max-width: 100px;
    cursor: pointer;
    text-align: center;
  }

  svg {
    width: 25px;
  }

  .done {
  text-decoration: line-through;
  }

  .input {
    width: 350px
  }

  .filterbar {
    display: flex;
    justify-content: space-between;
  }

  .filterbuttons {
    display: grid;
    grid-template-columns: auto auto auto;
    grid-column-gap: 10px;
    font-size:15px
  }

  .new-todo-button {
    display: inline-block;
    height: 65px;
    right: 13px;
    transition: opacity .3s ease;
    background-image: url("data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMTIgMmM1LjUxNCAwIDEwIDQuNDg2IDEwIDEwcy00LjQ4NiAxMC0xMCAxMC0xMC00LjQ4Ni0xMC0xMCA0LjQ4Ni0xMCAxMC0xMHptMC0yYy02LjYyNyAwLTEyIDUuMzczLTEyIDEyczUuMzczIDEyIDEyIDEyIDEyLTUuMzczIDEyLTEyLTUuMzczLTEyLTEyLTEyem02IDEzaC01djVoLTJ2LTVoLTV2LTJoNXYtNWgydjVoNXYyeiIvPjwvc3ZnPg==");
    background-repeat: no-repeat;
    background-position: center;
    grid-column:2;
    grid-row:1;
  }
</style>
