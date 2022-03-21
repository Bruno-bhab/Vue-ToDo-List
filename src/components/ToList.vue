<template>
  <div class="bg-secondary h-100 w-100 d-flex justify-content-center">
    <div class="w-50 mt-5">
      <h1 class="text-center mb-5">Vue To-Do list</h1>
      <div class="d-flex justify-content-center mb-5">
        <button type="button" class="btn my-btn" :class="[ showList ? 'btn-outline-dark' : 'btn-dark' ]" @click="showHideToggle">Show/Hide List</button>
      </div>
      
      <div v-if="showList">
        <div class="input-group mb-3">
          <input type="text" class="form-control bg-dark border border-dark text-light" placeholder="New Task" v-model="newTask" @keydown.enter="insertItem">
          <button class="btn btn-dark" type="button" @click="insertItem">Insert</button>
        </div>
        <div v-if="alert" class="position-absolute top-0 start-0 bg-dark w-100 h-100 opacity-50">
          <div class="alert alert-danger mt-3 position-absolute top-50 start-50 translate-middle" style="opacity: 1" role="alert" v-if="alert">           
          Task cannot be empty
        </div>
        </div>
        
        <table class="table table-hover" v-if="tasks[0]">
          <thead class="table-dark">
            <tr>
              <th scope="col" class="none" style="width: 50px">id</th>
              <th scope="col" class="none">Task</th>
              <th scope="col" class="none" style="width: 100px">Status</th>
              <th scope="col" class="none" style="width: 50px">#</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
              <td class="none">{{ index }}</td>
              <td class="pointer" @click="checked(index)"><span :class="{'checked' : task.checked}">{{ task.name }}</span></td>
              <td class="none">{{ task.status }}</td>
              <td><span class="pointer" @click="deleteTask(index)">X</span></td>
            </tr>
          </tbody>
        </table>
        <h2 class="text-center fs-5 fw-normal" v-else>
          No tasks
        </h2>
      </div>
      
      <h2 class="text-center fs-5 fw-normal" v-else>
        Click the button above to show your tasks
      </h2>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      tasks: [],
      showList: false,
      newTask: '',
      alert: false
    }
  },

  methods: {

    showHideToggle(){
      this.showList = !this.showList
    },

    insertItem(){
      if(this.newTask !== ''){
        let currentTask = {
          name: this.newTask,
          status: 'To do',
          checked: false
        }
        this.tasks.push(currentTask)
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
        this.newTask = ''
      }else{
        this.alert = true
        setInterval(() => this.alert = false, 1000)
      }
      
      
    },

    deleteTask(index){
      this.tasks.splice(index, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },

    getTasks(){
      this.tasks = JSON.parse(localStorage.getItem("tasks") || "[]")
    },

    checked(index){
      this.tasks[index].checked = !this.tasks[index].checked
      if(this.tasks[index].status == "To-Do"){
        this.tasks[index].status = "Complete"
      }else{
        this.tasks[index].status = "To-Do"
      }
      
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    }


  },

  mounted(){
    this.getTasks()
    console.log(typeof this.tasks)
  }
}
</script>

<style scoped>
  .pointer{
    cursor: pointer;
  }

  .none{
    cursor: default;
  }

  .checked{
    text-decoration: line-through;
  }
</style>