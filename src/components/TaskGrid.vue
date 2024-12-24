<template>
  <div class="TaskGrid">
    <template v-if="tasks.length">
      <TaskTemplete
        v-for="(task, i) in tasks"
        :key="task.name"
        @taskDeleted="$emit('taskDeleted', i)"
        @taskStateChanged="$emit('taskStateChanged',i)"
        :task="task"
      ></TaskTemplete>
    </template>
    <p v-else class="no-task">Sem Tarefas (>‿◠)✌</p>
  </div>
</template>

<script>
import TaskTemplete from "./TaskTemplete.vue";

export default {
  components: { TaskTemplete },
  props: {
    tasks: { type: Array, require: true },
  },
};
</script>

<style>
    .TaskGrid {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }

    .TaskGrid .task {
        margin: 10px;
    }

    .no-task {
        color:#aaa;
        font-size: 1.7rem;
    }


</style>