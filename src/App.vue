<script setup>
  import {reactive} from 'vue';

  const estado = reactive({ 
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
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

  const cadastraTarefa = () => { 
    const tarefaNova = { 
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }

  const getTarefasPendentes = () => { 
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => { 
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltrada = () => { 
    const { filtro } = estado;

    switch(filtro) { 
      case 'pendentes': 
        return getTarefasPendentes();
      case 'finalizadas': 
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
  </div>

  <form @submit.prevent="cadastraTarefa" > <!--.prevent serve o mesmo propósito do e.preventDefault()-->
    <div class="row">
      <div class="col">
        <input @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </div>
      <div class="col-md-2">
        <select @change="evento => estado.filtro = evento.target.value" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>

  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in getTarefasFiltrada()">
      <input :value="estado.tarefaTemp" @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="text" class="checkbox">
        <label :class="{done: tarefa.finalizada === true}" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </input>
  </li>
  </ul>
</template>

<style scoped>
  .done { 
    text-decoration: line-through;
  }
</style>
