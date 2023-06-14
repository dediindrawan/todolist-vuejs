<template>
<!-- wrapper container start -->
  <div class="item-task m-4">
  
    <!-- task group container start -->
    <div class="container m-auto">
    
      <!-- title and notif -->
      <h1 v-html="rawHtml" class="mb-4"></h1>
      <p class="text-muted" v-if="isCreating && tasks.length == 1">{{ notif }}</p>
      <p class="text-muted" v-else-if="!isCreating && tasks.length == 1">Buat task baru</p>

      <!-- task list -->
      <div v-for="task of tasks" :key="task.id" class="task-list">
        <div class="wrapper d-flex p-2" v-if="task.title && task.description">
          <input type="checkbox" name="status" id="task" class="mr-3" :checked="tasks.isDone" v-if="task.title && task.description" @change="checkTask(task.id)" />
          <div class="d-flex flex-column">
            <div :class="{ 'task-title': true, isCompleted: task.isDone }">
              {{ task.title }}
            </div>
            <div class="task-description small text-muted">
              {{ task.description }}
            </div>
            <a href="#" class="delete-button text-danger" v-if="task.title && task.description" @click="deleteTask(task.id)">Delete Task</a>
          </div>
        </div>
      </div>

      <!-- action control areas -->
      <div class="action d-flex justify-content-between">
        <a href="#" class="add-button" v-if="isCreating" @click="isCreating = !isCreating">Add Task</a>

        <div class="add-card col-md-8 px-0" v-else>
          <div class="card border-0 mb-2">
            <div class="card-body d-flex flex-column p-0">
              <input v-model="titleValue" class="form-control border-1 mb-2" placeholder="Task Title" type="text" />
              <textarea v-model="descriptionValue" class="form-control border-1 small" placeholder="Task Description" rows="3"></textarea>
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2 mr-2 col-sm-2" @click="saveTask">Save</button>
            <button class="btn btn-outline-secondary col-sm-2" @click="isCreating = !isCreating">Cancel</button>
          </div>
        </div>
      </div>
      
    </div>
    <!-- task group container end -->
    
  </div>
  <!-- wrapper container end -->
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';
export default {
  data() {
    return {
      // userface default display
      rawHtml: `Todo List <small style="font-size: 1rem;color:grey">WebApplication</small>`,
      notif: 'Belum ada task',
      tasks: [
        {
          isDone: false,
        },
      ],
      isCreating: false,
      titleValue: '',
      descriptionValue: '',
    };
  },
  methods: {
    // create task
    saveTask() {
      this.tasks.unshift({
        id: this.tasks.length + 1,
        title: this.titleValue,
        description: this.descriptionValue,
        isDone: false,
      });
    },
    // delete task
    deleteTask(index) {
      const taskIndex = this.tasks.findIndex((task) => task.id === index);

      if (taskIndex !== -1) {
        this.tasks.splice(taskIndex, 1);
      }
    },
     // check task
    checkTask(index) {
      const taskIndex = this.tasks.findIndex((task) => task.id === index);

      if (taskIndex !== -1) {
        this.tasks[taskIndex].isDone = !this.tasks[taskIndex].isDone;
      }
    },
  },
};
</script>

<style scoped>
.task-list {
  margin: 0.75em auto;
  border-radius: 5px;
  background: linear-gradient(to bottom right, rgba(0, 85, 255, 0.1), rgba(0, 116, 224, 0.2));
}
textarea {
  resize: none;
}
/* add new class isCompleted for list element when checkbox change is active */
.isCompleted {
  font-style: italic;
  text-decoration: line-through;
  opacity: 0.75;
}
</style>
