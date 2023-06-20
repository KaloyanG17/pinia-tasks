<template>
  <main>
    <header>
      <img src="./assets/pinia-logo.png" alt="Pinia logo">
      <h1>Pinia tasks</h1>
    </header>

    <div class="new-task-form">
      <TaskForm />
    </div>

    <nav class="filter">
      <button @click="filter = 'all'" >All tasks</button>
      <button @click="filter = 'favs'" >Fav tasks</button>
    </nav>

    <div class="loading" v-if="loading">Loading tasks ...</div>

    <div v-if="filter === 'all'" class="task-list">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task"/>
      </div>
    </div>

    <div  v-if="filter === 'favs'" class="task-list">
      <p>You have {{ favCount }} favs left to do</p>
      <div v-for="task in favs">
        <TaskDetails :task="task"/>
      </div>
    </div>

    <div class="filter">
      <button @click="$reset" >Reset</button>
    </div>
  </main>
</template>

<script>
import { useTaskStore } from './stores/TaskStore'
import TaskDetails from './components/TaskDetails.vue'
import TaskForm from './components/TaskForm.vue';
import { ref } from 'vue';
import { storeToRefs } from 'pinia';

export default {
  components: { TaskDetails, TaskForm},
  setup(){
    const taskStore = useTaskStore();
    const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore)
    taskStore.getTasks()
    const filter = ref('all')

    return {
      taskStore, filter, tasks, loading, favs, totalCount, favCount 
    }
  }
}
</script>

<style lang="scss" scoped>

</style>