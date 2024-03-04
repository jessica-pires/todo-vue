<script setup>
import { reactive } from 'vue';


const estado = reactive({
  filtro: 'todas',
  tarefaTemporaria:'',
  tarefas: [
    {
      titulo: 'Estudar react',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Entregar trabalho AVA',
      finalizada: true,
    },
  
  ]

})

  const getTarefasPendentes = ()=>{
    return estado.tarefas.filter(tarefa=> !tarefa.finalizada)
  }

  const getTarefasFinalizadas = ()=>{
    return estado.tarefas.filter(tarefa=> tarefa.finalizada)
  }

  const getTarefasFiltradas = ()=>{
    const { filtro } = estado;
    
    switch (filtro){
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas
    }
  }

  const cadastraTarefa = () =>{
    const tarefaNova = {
      titulo: estado.tarefaTemporaria,
      finalizada: false
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemporaria = '';

  }


</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você possui {{getTarefasPendentes().length}} tarefas pendentes</p>
    </header>
  
  <form @submit.prevent="cadastraTarefa">
    <div class="row">
      <div class="col">
        <input :value="estado.tarefaTemporaria" type="text" @change="evento => estado.tarefaTemporaria =evento.target.value " required placeholder="Digite aqui a tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="event => estado.filtro = event.target.value " class="form-control">
          <option value="todos">
            Todas tarefas
          </option>
          <option value="pendentes">
            Tarefas pendentes
          </option>
          <option value="finalizadas">
            Finalizadas
          </option>
        </select>
      </div>
    </div>
  </form>
  

  <!-- listagem -->
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
      <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
      <label :class="{ done: tarefa.finalizada === true}" class="ms-3" :for="tarefa.titulo">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
</div>
</template>

<style scoped>
.done{
  text-decoration: line-through;
}

</style>

<!-- bootstrap baixado pelo link=> npm i bootstrap@5.3.3 -->