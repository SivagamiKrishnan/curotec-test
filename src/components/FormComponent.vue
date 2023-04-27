<template>
  <div class="form-wrapper">
    <div class="form-title">
        <span>Tasks </span>
        <span>{{tasksCount}}</span>
    </div>
    {{tasks}}
    <div>
      <input class="form-input" type="text" v-model="task" placeholder="New Tasks"/>
      <button class="btn" @click="addTask(task)">
        <span>&plus;</span>
        <span>Add</span>
      </button>
    </div>
    <div class="todo-list">
      <ul v-for="(taskItem, index) in tasks" :key="taskItem">
        <div
          @click="toggleStatus(taskItem, index)"
         
        >
        {{task.color}}
          <li class="form-li"
             :class="taskItem.color"
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
    }
  },
  data() {
    return {
      task: '',
      tasks: this.getTaskCookie() || [],
      isCompleted: []
    }
  },
  methods:{
    addTask(taskItem) {
      let  taskObj = {
        title: taskItem,
        status: "incomplete",
        color: "grey-bg"
      }
      console.log("taskType",this.tasks);
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
    toggleStatus(task){
      task.status = ( task.status === "incomplete")? "completed" : "incomplete";
      task.color = (task.status === "completed") ? 'green-bg' : 'grey-bg';
      let tempTasksObj = Object.assign({}, this.tasks);
      console.log(tempTasksObj);
      this.SetTasksInCookie(tempTasksObj);
    },
    SetTasksInCookie(obj) {
      console.log("obj", obj);
      let cookieTaskStr = JSON.stringify(obj);
      document.cookie = "tasks=";
      document.cookie = "tasks="+ cookieTaskStr;
    },
    getTaskCookie(){
      let cookieStr = ('; '+document.cookie).split(`; tasks=`).pop().split(';')[0];
      return (cookieStr) ? JSON.parse(cookieStr) : [];
    }
  },
  mounted() {
    this.tasks = this.getTaskCookie() || [];
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
