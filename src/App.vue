<script setup>
import { ref, computed } from 'vue'
const tarefas = ref([
  { id: 1, desc: 'Prova de Geografia', status: 'pendente' },
  { id: 2, desc: 'Prova de história', status: 'concluida' },
  { id: 3, desc: 'Trabalho DevWeb', status: 'pendente' },
])

const novaTarefa = ref('')
const posicaoAlterar = ref(-1)
const filtro = ref('')

const tarefasPendentes = computed(() => {
  return tarefas.value.filter((t) => t.status == 'pendente').length
})
const tarefasConcluidas = computed(() => {
  return tarefas.value.filter((t) => t.status == 'concluida').length
})

const tarefasFiltradas = computed(() => {
  if (filtro.value.trim() == '') {
    return tarefas.value;
  }
  else {
    return tarefas.value.filter(t => t.desc.includes(filtro.value))
  }
})

function ordenar (){
  tarefas.value.sort((a, b) => a.desc.localeCompare(b.desc, 'pt-BR'))
}
function addTarefa() {
  if (posicaoAlterar.value == -1) {
    let maiorId = Math.max(...tarefas.value.map((item) => item.id))
    tarefas.value.push({
      id: maiorId + 1,
      desc: novaTarefa.value,
      status: 'pendente',
    })
  } else {
    tarefas.value[posicaoAlterar.value].desc = novaTarefa.value
    posicaoAlterar.value = -1
  }

  novaTarefa.value = ''
}

function deleteTarefa(item) {
  const posicao = tarefas.value.findIndex((t) => t.id === item.id)
  tarefas.value.splice(posicao, 1)
}

function editTarefa(item) {
  posicaoAlterar.value = tarefas.value.findIndex((t) => t.id === item.id)
  novaTarefa.value = tarefas.value[posicaoAlterar.value].desc
}

function marcarConcluida(id) {
  const posicao = tarefas.value.findIndex((t) => t.id == id)
  if (tarefas.value[posicao].status === 'concluida') {
    tarefas.value[posicao].status = 'pendente'
  } else if (tarefas.value[posicao].status == 'pendente') {
    tarefas.value[posicao].status = 'concluida'
  }
}
</script>

<template>
  <div class="container">
    <input type="text" v-model="novaTarefa" @keyup.enter="addTarefa" />
    <button @click="addTarefa">Adicionar</button>
    <button @click.prevent="ordenar()">Ordenar</button>

    <ul>
      <li v-for="item in tarefasFiltradas" :key="item.id"
      
      >
        <span @click="marcarConcluida(item.id)" :class="{ concluida: item.status == 'concluida' }">
          {{ item.desc }}
        </span>
        <span>
          <a href="#" @click.prevent="deleteTarefa(item)">Deletar</a>
          <a href="#" @click.prevent="editTarefa(item)">Editar</a>
        </span>
      </li>
    </ul>
    <div class="pendente">
      <span>Pendentes: {{ tarefasPendentes }}</span>
      <span>Pendentes: {{ tarefasConcluidas }}</span>
    </div>
    <span><input type="text" v-model="filtro" placeholder="Filtrar"></span>
  </div>
</template>

<style scoped>
div.container {
  max-width: 500px;
  margin: 50px auto;
  background-color: #1e293b;
  padding: 30px;
  border-radius: 12px;
  text-align: center;
  color: white;
}
li {
  cursor: pointer;
  color: white;
  margin: 1vw;
}
.concluida {
  text-decoration: line-through;
}
a {
  margin-left: 10px;
  color: #608ea1;
  text-decoration: none;
  font-size: 14px;
}

.pendente {
  margin-top: 20px;
  display: flex;
  justify-content: space-around;
  font-weight: bold;
}
</style>
