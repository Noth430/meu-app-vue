<template>
  <div class="app">
    <h1>📝 Tutorial App</h1>
    <p class="subtitle">{{ tarefasPendentes }} pendente(s) de {{ tarefas.length }}</p>

    <!-- Campo para adicionar -->
    <div class="input-area">
      <input
        v-model="novaTarefa"
        @keyup.enter="adicionar"
        placeholder="Digite uma nova tarefa..."
      />
      <button @click="adicionar" class="btn-add">+ Adicionar</button>
    </div>

    <!-- Filtros -->
    <div class="filtros">
      <button
        v-for="f in ['todas', 'pendentes', 'concluidas']"
        :key="f"
        @click="filtro = f"
        :class="['btn-filtro', { ativo: filtro === f }]"
      >
        {{ f === 'todas' ? '📋 Todas' : f === 'pendentes' ? '⏳ Pendentes' : '✅ Concluídas' }}
      </button>
    </div>

    <!-- Lista de tarefas -->
    <div v-if="tarefasFiltradas.length === 0" class="vazio">
      🎉 Nenhuma tarefa por aqui!
    </div>

    <div
      v-for="tarefa in tarefasFiltradas"
      :key="tarefa.id"
      :class="['tarefa', { concluida: tarefa.feita }]"
    >
      <div class="tarefa-esquerda" @click="tarefa.feita = !tarefa.feita">
        <span class="check">{{ tarefa.feita ? '✅' : '⬜' }}</span>
        <span class="tarefa-texto">{{ tarefa.texto }}</span>
      </div>
      <button @click="remover(tarefa.id)" class="btn-remover">🗑️</button>
    </div>

    <!-- Limpar concluídas -->
    <button
      v-if="tarefas.some(t => t.feita)"
      @click="limparConcluidas"
      class="btn-limpar"
    >
      🧹 Limpar concluídas
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const novaTarefa = ref('')
const filtro = ref('todas')
let proximoId = 4

const tarefas = ref([
  { id: 1, texto: 'Aprender Vue.js', feita: true },
  { id: 2, texto: 'Criar meu primeiro componente', feita: false },
  { id: 3, texto: 'Dominar o mundo', feita: false },
])

const tarefasPendentes = computed(() =>
  tarefas.value.filter(t => !t.feita).length
)

const tarefasFiltradas = computed(() => {
  if (filtro.value === 'pendentes') return tarefas.value.filter(t => !t.feita)
  if (filtro.value === 'concluidas') return tarefas.value.filter(t => t.feita)
  return tarefas.value
})

function adicionar() {
  const texto = novaTarefa.value.trim()
  if (!texto) return
  tarefas.value.push({ id: proximoId++, texto, feita: false })
  novaTarefa.value = ''
}

function remover(id) {
  tarefas.value = tarefas.value.filter(t => t.id !== id)
}

function limparConcluidas() {
  tarefas.value = tarefas.value.filter(t => !t.feita)
}
</script>

<style scoped>
.app {
  max-width: 500px;
  margin: 40px auto;
  padding: 30px;
  font-family: 'Segoe UI', sans-serif;
  background: #1a1a2e;
  border-radius: 20px;
  color: #e6e6e6;
}

h1 {
  text-align: center;
  font-size: 28px;
  margin-bottom: 4px;
}

.subtitle {
  text-align: center;
  color: #888;
  font-size: 14px;
  margin-bottom: 24px;
}

.input-area {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

input {
  flex: 1;
  padding: 12px 16px;
  border-radius: 10px;
  border: 2px solid #333;
  background: #16213e;
  color: #fff;
  font-size: 14px;
  outline: none;
  transition: border 0.2s;
}

input:focus {
  border-color: #0d79d1;
}

.btn-add {
  padding: 12px 20px;
  background: #0d79d1;
  color: white;
  border: none;
  border-radius: 10px;
  font-weight: 600;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s;
  
}

.btn-add:hover {
  background: #0d79d1;
}

.filtros {
  display: flex;
  gap: 6px;
  margin-bottom: 20px;
}

.btn-filtro {
  flex: 1;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #333;
  background: transparent;
  color: #888;
  font-size: 12px;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-filtro.ativo {
  background: #0d79d1;
  color: white;
  border-color: #0d79d1;
}

.tarefa {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  margin-bottom: 8px;
  background: #16213e;
  border-radius: 10px;
  border: 1px solid #333;
  transition: all 0.2s;
}

.tarefa:hover {
  border-color: #0d79d1;
}

.tarefa.concluida {
  opacity: 0.5;
}

.tarefa.concluida .tarefa-texto {
  text-decoration: line-through;
}

.tarefa-esquerda {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  flex: 1;
}

.check {
  font-size: 18px;
}

.tarefa-texto {
  font-size: 14px;
}

.btn-remover {
  background: none;
  border: none;
  font-size: 16px;
  cursor: pointer;
  opacity: 0.4;
  transition: opacity 0.2s;
}

.btn-remover:hover {
  opacity: 1;
}

.btn-limpar {
  width: 100%;
  padding: 10px;
  margin-top: 16px;
  background: transparent;
  border: 1px dashed #555;
  color: #888;
  border-radius: 10px;
  cursor: pointer;
  font-size: 13px;
  transition: all 0.2s;
}

.btn-limpar:hover {
  border-color: #f85149;
  color: #f85149;
}

.vazio {
  text-align: center;
  padding: 40px;
  color: #555;
  font-size: 16px;
}
</style>