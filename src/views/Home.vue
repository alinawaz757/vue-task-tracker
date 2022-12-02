<template>
  <AddTask v-if="showForm" @add-task="addTask" />
  <Tasks
    :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
  />
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default {
  name: 'Home',
  props: {
    showForm: Boolean,
  },
  data() {
    return {
      tasks: [],
    };
  },
  components: {
    Tasks,
    AddTask,
  },
  methods: {
    async deleteTask(id) {
      if (confirm('are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('error while deleting');
      }
    },
    async addTask(values) {
      const res = await fetch('api/tasks/', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(values),
      });
      const data = await res.json();
      this.tasks.push(data);
    },
    async toggleReminder(id) {
      const task = this.tasks.find((task) => id === task.id);
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify({ ...task, reminder: !task.reminder }),
      });
      if (res.status === 200) {
        const data = await res.json();
        this.tasks = this.tasks.map((task) => {
          if (task.id === id) task.reminder = data.reminder;
          return task;
        });
      } else {
        alert('error while updating');
      }
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
  async mounted() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
