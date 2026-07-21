<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import ListaTarefas from "./components/ListaTarefas.vue";

interface Tarefa {
  id: number;
  texto: string;
  concluida: boolean;
}

interface Lista {
  id: number;
  titulo: string;
  tarefas: Tarefa[];
}

const listas = ref<Lista[]>([]);
const tituloLista = ref("");

function criarLista() {
  if (tituloLista.value.trim() === "") return;

  listas.value.push({
    id: Date.now(),
    titulo: tituloLista.value,
    tarefas: []
  });

  tituloLista.value = "";
}

function excluirLista(id: number) {
  listas.value = listas.value.filter(lista => lista.id !== id);
}

onMounted(() => {
  const dados = localStorage.getItem("listas");

  if (dados) {
    listas.value = JSON.parse(dados);
  }
});

watch(
  listas,
  () => {
    localStorage.setItem("listas", JSON.stringify(listas.value));
  },
  { deep: true }
);
</script>

<template>
  <div class="container">

    <h1>📋 Organizador de Tarefas</h1>

    <p class="subtitulo">
      Crie várias listas e organize suas tarefas.
    </p>

    <div class="nova-lista">

      <input
        v-model="tituloLista"
        placeholder="Nome da lista"
      />

      <button @click="criarLista">
        Criar Lista
      </button>

    </div>

    <ListaTarefas
      v-for="lista in listas"
      :key="lista.id"
      :lista="lista"
      @excluir="excluirLista"
    />

  </div>
</template>

<style scoped>

.container{

    max-width:1100px;

    margin:auto;

}

h1{

    color:white;

    text-align:center;

    margin-bottom:10px;

}

.subtitulo{

    text-align:center;

    color:white;

    margin-bottom:35px;

}

.nova-lista{

    display:flex;

    gap:15px;

    margin-bottom:30px;

}

.nova-lista input{

    flex:1;

    padding:14px;

    border:none;

    border-radius:10px;

    font-size:16px;

}

.nova-lista button{

    background:#35495e;

    color:white;

    border:none;

    border-radius:10px;

    padding:14px 20px;

    cursor:pointer;

}

.nova-lista button:hover{

    background:#243342;

}

@media(max-width:700px){

.nova-lista{

flex-direction:column;

}

}

</style>
