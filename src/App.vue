<template>
  <div class="container" style="max-width: 600px">
    <h2 class="text-center mt-5">ToDo List</h2>
    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
        @keypress="(e) => {if(e.key == 'Enter') submitTask()}"
      />
      <button class="btn btn-outline-success" @click="submitTask" @keypress="submitTask">
        SUBMIT
      </button>
    </div>
    <table class="table table-striped mt-5">
      <thead>
        <tr>
          <th scope="col" style="text-center">Task</th>
          <th scope="col" style="text-center">Status</th>
          <th scope="col" class="text-center">Delete</th>
          <th scope="col" class="text-center">Edit</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              class="pointer noselect"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
              {{ capitalizeFirstChar(task.status) }}
            </span>
          </td>
          <td class="text-center">
            <div @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
          <td class="text-center">
            <div @click="editTask(index)">
              <p class="fa fa-pen pointer"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "someThing",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      status: ["to-do", "in-progress", "finished"],

      tasks: [
        
      ],
    };
  },

  methods: {
  
    capitalizeFirstChar(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

 
    changeStatus(index) {
      let newIndex = this.status.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.status[newIndex];
    },


    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

 
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },


    submitTask() {
      if (this.task.length === 0) return;

      //Update task
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {

     //Add new task
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    }
  }
}
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.line-through {
  text-decoration: line-through;
}
</style>