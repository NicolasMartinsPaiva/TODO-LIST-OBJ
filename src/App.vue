<script setup>
import { ref } from 'vue';
const tarefas = ref([
  { id: 1, desc: 'Prova de Geografia', status: 'pendente'},
  { id: 2, desc: 'Prova de história', status: 'concluida'},
  { id: 3, desc: 'Trabalho DevWeb', status: 'pendente'}
]);

const novaTarefa = ref('');
const posicaoAlterar = ref(-1)

function addTarefa () {

  if (posicaoAlterar.value == -1) {
    let maiorId = Math.max(...tarefas.value.map(item => item.id));
    tarefas.value.push({
    id: maiorId + 1,
    desc: novaTarefa.value, status: 'pendente'
  });
  }
  else {
    tarefas.value[posicaoAlterar.value].desc = novaTarefa.value
    posicaoAlterar.value = -1;
  }
  
  
  novaTarefa.value = '';
}

function deleteTarefa (item) {
  const posicao = tarefas.value.findIndex(t => t.id === item.id);
  tarefas.value.splice(posicao, 1);
}
function editTarefa (item) {
  posicaoAlterar.value = tarefas.value.findIndex(t => t.id === item.id);
  novaTarefa.value = tarefas.value[posicaoAlterar.value].desc;
}
</script>

<template>
  <div class="container">
    <input type="text" v-model="novaTarefa" @keyup.enter="addTarefa">
    <button @click="addTarefa">Adicionar</button>
    <ul>
      <li v-for="item in tarefas" :key="item.id">
        {{ item.desc }}
        <span>
        <a href="#" @click.prevent="deleteTarefa(item)">Deletar</a>
        <a href="#" @click.prevent="editTarefa(item)">Editar</a>
      </span>

      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>