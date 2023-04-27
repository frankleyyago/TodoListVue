<script setup>
import { reactive } from 'vue';

const state = reactive({
  filter: 'all',
  tempTask: '',
  tasks: [
    {
      title: 'Estudar ES6',
      finished: false,
    },
    {
      title: 'Estudar SASS',
      finished: false,
    },
    {
      title: 'Ir para a academia',
      finished: true,
    }
  ]
})

const getPendentTasks = () => {
  return state.tasks.filter(task => !task.finished)
}

const getFinishedTasks = () => {
  return state.tasks.filter(task => task.finished)
}

const getFilteredTasks = () => {
  const { filter } = state

  switch (filter) {
    case 'pendent':
      return getPendentTasks()
    case 'finished':
      return getFinishedTasks()
    default:
      return state.tasks
  }
}

  const addTask = () => {
    const newTask = {
      title: state.tempTask,
      finished: false,
    }
    state.tasks.push(newTask)
    state.tempTask = ''
  }
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getPendentTasks().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="addTask">
      <div class="row">
        <div class="col">
          <input :value='state.tempTask' @change="event => state.tempTask = event.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="event => state.filter = event.target.value" class="form-control">
            <option value="all">Todas as tarefas</option>
            <option value="pendent">Pendentes</option>
            <option value="finished">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="task in getFilteredTasks()">
        <input @change="event => task.finished = event.target.checked" :checked="task.finished" :id="task.title" type="checkbox">
        <label :class="{ done: task.finished }" class="ms-3" :for="task.title">
          {{ task.title }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>

  .done {
    text-decoration: line-through;
  }

</style>
