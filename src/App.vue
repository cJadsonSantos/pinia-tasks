<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo">
      <h1>Pinia Tasks</h1>
    </header>
    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm/>
    </div>
    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Fav tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>
    <!-- task list-->
    <div class="task-list" v-if="filter === 'all'">
      <p>Your have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task"/>
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>Your have {{ favCount }} favs left to do</p>

      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task"/>
      </div>
    </div>

    <button @click="taskStore.$reset()">reset state</button>
  </main>
</template>

<script>
import TaskDetails from "@/components/TaskDetails.vue";
import {useTaskStore} from "@/stores/TaskStore";
import {ref} from "vue";
import TaskForm from "@/components/TaskForm.vue";
import {storeToRefs} from "pinia";

export default {
  components: {TaskForm, TaskDetails},
  setup() {
    const taskStore = useTaskStore()

    const {tasks, loading, favs, totalCount, favCount} = storeToRefs(taskStore)
    // fetch tasks
    taskStore.getTasks()

    const filter = ref('all')
    return {taskStore, filter, tasks, loading, favs, totalCount, favCount}
  }
}
</script>
