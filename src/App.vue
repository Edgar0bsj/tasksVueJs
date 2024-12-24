<template>
  <div id="app">
    <h1>Tarefas ✍</h1>
    <TaskProgress :progress="progress" />
    <NewTask @taskAdded="addTask" />
    <TaskGrid
      :tasks="tasks"
      @taskDeleted="deleteTask"
      @taskStateChanged="toggleTaskState"
    />
  </div>
</template>

<script>
import NewTask from "./components/NewTask.vue";
import TaskGrid from "./components/TaskGrid.vue";
import TaskProgress from "./components/TaskProgress.vue";

export default {
  components: { NewTask, TaskGrid, TaskProgress },
  data() {
    return {
      tasks: [],
    };
  },
  computed: {
    progress() {
      const total = this.tasks.length;
      const done = this.tasks.filter((t) => !t.pending).length;
      return Math.round((done / total) * 100) || 0;
    },
  },
  watch: {
    tasks: {
      deep: true,
      handler() {
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      },
    },
  },
  methods: {
    addTask(event) {
      return new Promise((resolve, reject) => {
        try {
          if (event.name == "") {
            console.log("Empty task");
          } else {
            const sameName = (t) => t.name === event.name;
            const reallyNew = this.tasks.filter(sameName).length == 0;
            if (reallyNew) {
              this.tasks.push({
                name: event.name,
                pending: event.pending || true,
              });
              resolve(console.log("Task added"));
            }
          }
        } catch (error) {
          reject(console.error(error));
        }
      });
    },
    deleteTask(i) {
      this.tasks.splice(i, 1);
    },
    toggleTaskState(i) {
      this.tasks[i].pending = !this.tasks[i].pending;
    },
  },
  created() {
    const json = localStorage.getItem("tasks");
    this.tasks = JSON.parse(json) || [];
  },
};
</script>

<style>
body {
  font-family: "Lato", sans-serif;
  /* background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115)); */
  background: linear-gradient(to right, #000000, #434343);
  color: aliceblue;
}
#app {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app h1 {
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
}
</style>
