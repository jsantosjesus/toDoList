<template>
  <div class="m-4">
    <form @submit.prevent="salvarTarefa">
      <div class="mb-3">
        <label class="form-label">Nova tarefa</label>
        <div class="d-flex flex-row">
          <input
            v-model="tarefa"
            required
            type="text"
            class="form-control me-2"
            placeholder="ex: Tirar o lixo"
          />
          <button type="submit" class="btn btn-primary">Salvar</button>
        </div>
      </div>
    </form>
  </div>
  <div class="m-4">
    <h1>Tarefas para fazer</h1>
    <p v-if="tarefas.length == 0">Você não tem nenhuma tarefa</p>
    <ul
      v-if="tarefas.length > 0"
      class="list-group mb-1"
      v-for="(tarefa, i) in tarefas"
      :key="i"
    >
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        {{ tarefa }}
        <div>
          <button
            type="button"
            title="excluir tarefa"
            class="btn btn-success m-1"
            @click="marcarConcluida(i)"
          >
            <i class="fas fa-check"></i>
          </button>

          <button
            type="button"
            title="Marcar tarefa como concluída"
            class="btn btn-danger m-1"
            @click="excluirTarefa(i, tarefas)"
          >
            <i class="fas fa-close"></i>
          </button>
        </div>
      </li>
    </ul>
  </div>
  <div class="m-4">
    <h1>Tarefas concluídas</h1>
    <p v-if="tarefasConcluidas.length == 0">Você não concluiu nenhuma tarefa</p>
    <ul
      v-if="tarefasConcluidas.length > 0"
      class="list-group mb-1"
      v-for="(tarefa, i) in tarefasConcluidas"
      :key="i"
    >
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        {{ tarefa }}
        <div>
          <button
            type="button"
            title="Refazer tarefa"
            class="btn btn-primary m-1"
            @click="refazerTarefa(i, tarefasConcluidas)"
          >
            <i class="fas fa-recycle"></i>
          </button>
          <button
            type="button"
            title="Excluir tarefa"
            class="btn btn-danger m-1"
            @click="excluirTarefa(i, tarefasConcluidas)"
          >
            <i class="fas fa-close"></i>
          </button>
          
        </div>
      </li>
    </ul>
  </div>
  <div class="m-4">
    <h1>Tarefas excluidas</h1>
    <p v-if="tarefasExcluidas.length == 0">Sua lixeira está vazia</p>
    <ul
      v-if="tarefasExcluidas.length > 0"
      class="list-group mb-1"
      v-for="(tarefa, i) in tarefasExcluidas"
      :key="i"
    >
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        {{ tarefa }}
        <div>
          <button
            type="button"
            title="Reciclar tarefa"
            class="btn btn-primary m-1"
            @click="refazerTarefa(i, tarefasExcluidas)"
          >
            <i class="fas fa-recycle"></i>
          </button>
          <button
            type="button"
            title="Excluir tarefa"
            class="btn btn-danger m-1"
            @click="excluirTarefa(i, tarefasExcluidas)"
          >
            <i class="fas fa-close"></i>
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, reactive } from "vue";

const tarefa = ref();
const tarefas = reactive([]);
const tarefasConcluidas = reactive([]);
const tarefasExcluidas = reactive([]);

function salvarTarefa() {
  tarefas.unshift(tarefa.value);
  tarefa.value = "";
}

function marcarConcluida(index) {
  const tarefaConcluida = tarefas.splice(index, 1);
  tarefasConcluidas.unshift(...tarefaConcluida);
}

function excluirTarefa(index, lista) {
  if(lista == tarefasExcluidas && !confirm(`Confirme a exclusão permantente da tarefa ${lista[index]} ? `)){
    return
  }
  const tarefaExcluida = lista.splice(index, 1)
  if(lista != tarefasExcluidas){
    tarefasExcluidas.unshift(...tarefaExcluida);
  }
  
}

function refazerTarefa(index, lista) {
  const tarefaRefeita = lista.splice(index, 1);
  tarefas.unshift(...tarefaRefeita);
}
</script>
