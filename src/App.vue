<script setup>

import { pushScopeId, reactive } from 'vue'

const estado = reactive({
  filtro: 'todas',
  tarefas: [
    {
      titulo: 'Estudar Es6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    }
  ]
})


const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada);
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada);
}

const getTarefasfiltradas = () => {
  const {filtro } = estado;

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;  
  }
}

const cadastraTarefa = () => {
      const tarefaNova = {
        titulo: estado.tarefaTemp,
        finalizada: false,
      }
      estado.tarefas.push(tarefaNova);
      estado.tarefaTemp = '';
}

</script>
  
<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" type="text" required placeholder="Digite a terefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
      <select @change="evento => estado.filtro = evento.target.value" class="form-control">
        <option value="todas">Todas as tarefas</option>
        <option value="pendentes">Pendentes</option>
        <option value="finalizadas">Finalizadas</option>
      </select>
    </div>
    </div>
    </form>
    <ul  class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasfiltradas()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{done: tarefa.finalizada}" class="ms-3" :for="tarefa.titulo">
          {{tarefa.titulo}}
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
