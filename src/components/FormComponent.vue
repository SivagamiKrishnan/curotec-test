<template>
  <div class="form-wrapper">
    <div class="form-title">
        <span>Tasks </span>
        <span>{{tasksCount}}</span>
    </div>
    <div>
      <input class="form-input" type="text" v-model="task" placeholder="New Tasks"/>
      <button class="btn" @click="addTask(task)">
        <span>&plus;</span>
        <span>Add</span>
      </button>
    </div>
   
    <div class="todo-list">
      <ul v-for="(taskItem, index) in tasks" :key="taskItem">
        <div>
          <li class="form-li"
             @click="toggleStatus(taskItem, index)"
             :class="{
                'green-bg' : isCompleted[index],
                'grey-bg' : !isCompleted[index]
              }"
          >
            <span>
              {{taskItem.title}}
            </span>

          </li>
          <button class="btn btn-red" @click="deleteTask(taskItem)">X</button>
        </div>
      </ul> 
    </div> 
  </div>
</template>

<script>
export default {
  name: 'FormComponent',
  computed:{
    tasksCount(){
      return (this.tasks && this.tasks.length > 0)? this.tasks.length - 1 : 0;
    },
  },
  data() {
    return {
      task: '',
      tasks: [],
      isCompleted: []
    }
  },
  methods:{
    addTask(taskItem) {
      let  taskObj = {
        title: taskItem,
        status: "incomplete"
      }
      this.tasks.push(taskObj);
      let tempTasksObj = Object.assign({}, this.tasks);
      console.log(tempTasksObj);
      this.SetTasksInCookie(tempTasksObj);
      this.task= '';
    },
    deleteTask(taskItem) {
      let taskItemIndex = Object.values(this.tasks).indexOf(taskItem);
      this.tasks.splice(taskItemIndex, 1);
      let tempTasksObj = Object.assign({}, this.tasks);
      this.SetTasksInCookie(tempTasksObj);
    },
    toggleStatus(task, index){
      task.status = ( task.status === "incomplete")? "completed" : "incomplete";
      this.isCompleted[index] = (task.status === "completed");
      let tempTasksObj = Object.assign({}, this.tasks);
      console.log(tempTasksObj);
      this.SetTasksInCookie(tempTasksObj);
    },
    SetTasksInCookie(obj) {
      let cookieTaskStr = JSON.stringify(obj);
      document.cookie = "tasks=";
      document.cookie = "tasks="+ cookieTaskStr;
    }
  },
  mounted() {
    this.tasks = document.cookie("tasks") || [];
  }
}
</script>

<style scoped>
  .form-wrapper {
    display: block;
    width: 90%;
    margin: 0 auto;

      .form-title {
        font-size: 28px;
        margin-bottom: 20px;
      }
      .todo-list > ul {
        padding-left: 0;
      }
      .form-input, .form-li{
        width: 88%;
        padding: 10px;
        border: 1px solid grey;
        outline: 1px grey;
      }

      .form-li{
        list-style: none;
        display: inline-block;
      }

      .btn {
          padding: 12px;
          background-color: #2196F3;
          color: white;
          border: 1px solid #2196F3;
      }

      .btn-red {
        background-color: red;
      }

     .green-bg {
        background-color: #8bc34a52;
      }
      .grey-bg {
        background-color: #80808057;
      }


  }
 
</style>
