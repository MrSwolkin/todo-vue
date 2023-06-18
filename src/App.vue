<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

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
        titulo: 'Ir pro pilates', 
        finalizada: true,
      }
    ]
  })

  const cadastarTarefa = () => {
    const novaTarefa = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(novaTarefa);
    estado.tarefaTemp = '';
  }

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasfinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasfinalizadas();
      default:
        return estado.tarefas;

    }
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefasPendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-Filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastrar-tarefa="cadastarTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()" />
  </div>
</template>


