<template>
  <div class="container-fluid">
    <h1 v-html="rawHtml"></h1>
    <p v-if="!isCreating && tasks.length == 1">Belum ada task</p>
    <p v-else-if="!titleValue && !descriptionValue">Buat task baru</p>

    <div v-for="task of tasks" :key="task.id" class="task-list d-flex align-items-center mb-3">
      <input v-if="task.title || task.description" :checked="tasks[0].isDone" @change="checkTask(task)" type="checkbox" name="status" />

      <div class="list-wrapper col-md-12 ml-3">
        <div :class="{ 'task-title': true, completed: task.isDone }">
          <strong>{{ task.title }}</strong>
        </div>
        <div class="task-description">{{ task.description }}</div>
        <a href="#" @click="deleteTask(task.id)" v-if="task.title || task.description">Delete</a>
      </div>
    </div>

    <div class="action-control">
      <a href="#" v-if="!isCreating" @click="isCreating = !isCreating">Add Task</a>

      <div v-else class="task-form">
        <input v-model="titleValue" class="col-md-12 mb-3" type="text" name="Title" placeholder="Title" />
        <textarea v-model="descriptionValue" class="col-md-12 mb-3" name="Description" placeholder="Description"></textarea>

        <button @click="saveTask" class="btn btn-primary mr-3 col-sm-2">Save</button>
        <button @click="isCreating = !isCreating" class="btn btn-warning col-sm-2">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap-vue/dist/bootstrap-vue.css';
export default {
  data() {
    return {
      rawHtml: 'Todo List',
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
    saveTask() {
      this.tasks.unshift({
        id: this.tasks.length + 1,
        title: this.titleValue,
        description: this.descriptionValue,
      });
    },
    deleteTask(index) {
      const taskIndex = this.tasks.findIndex((task) => task.id === index);
      if (taskIndex !== -1) {
        this.tasks.splice(taskIndex, 1);
      }
    },
    checkTask(task) {
      task.isDone = !task.isDone;
    },
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  font-style: italic;
  color: grey;
}
.list-wrapper {
  background-color: rgba(102, 104, 109, 0.1);
}
</style>
