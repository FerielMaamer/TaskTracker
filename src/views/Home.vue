<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    async addTask(task) {
      const res = await fetch('https://ferielmaamer.live/api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })

      const data = await res.json()

      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`https://ferielmaamer.live/api/tasks/${id}`, {
          method: 'DELETE',
        })
        console.log(id)
        res.status === 204
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('Error deleting task')
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, isCompleted: !taskToToggle.isCompleted };

      const res = await fetch(`https://ferielmaamer.live/api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      });

      if (res.status === 204) {
        this.tasks = this.tasks.map((task) =>
          task.id === id ? { ...task, isCompleted: !task.isCompleted } : task
        );
      } else {
        alert('Error changing reminder');
      }
    },
    async fetchTasks() {
      const res = await fetch('https://ferielmaamer.live/api/tasks')
      const data = await res.json()
      console.log(data);
      return data
    },
    async fetchTask(id) {
      const res = await fetch(`https://ferielmaamer.live/api/tasks/${id}`)

      const data = await res.json()
      console.log(data);
      return data
    },
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
}
</script>
