<script setup lang="ts">
import { ref } from "vue";
import ItemTarefa from "./ItemTarefa.vue";

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

const props = defineProps<{
  lista: Lista;
}>();

const emit = defineEmits<{
  (e: "excluir", id: number): void;
}>();

const novaTarefa = ref("");

function adicionarTarefa() {
  if (novaTarefa.value.trim() === "") return;

  props.lista.tarefas.push({
    id: Date.now(),
    texto: novaTarefa.value,
    concluida: false
  });

  novaTarefa.value = "";
}

function removerTarefa(id: number) {
  props.lista.tarefas = props.lista.tarefas.filter(
    tarefa => tarefa.id !== id
  );
}

const totalConcluidas = () => {
  return props.lista.tarefas.filter(
    tarefa => tarefa.concluida
  ).length;
};
</script>

<template>

<div class="card">

    <div class="cabecalho">

        <h2>
            📋 {{ lista.titulo }}
        </h2>

        <button
            class="btnExcluirLista"
            @click="emit('excluir', lista.id)"
        >
            Excluir Lista
        </button>

    </div>

    <div class="nova">

        <input
            v-model="novaTarefa"
            placeholder="Digite uma tarefa"
            @keyup.enter="adicionarTarefa"
        >

        <button @click="adicionarTarefa">

            Adicionar

        </button>

    </div>

    <p class="contador">

        {{ totalConcluidas() }}
        de
        {{ lista.tarefas.length }}
        concluídas

    </p>

    <ul>

        <ItemTarefa

            v-for="tarefa in lista.tarefas"

            :key="tarefa.id"

            :tarefa="tarefa"

            @remover="removerTarefa"

        />

    </ul>

</div>

</template>

<style scoped>

.card{

    background:white;

    border-radius:18px;

    padding:25px;

    margin-bottom:30px;

    box-shadow:0 10px 25px rgba(0,0,0,.15);

    transition:.3s;

}

.card:hover{

    transform:translateY(-4px);

}

.cabecalho{

    display:flex;

    justify-content:space-between;

    align-items:center;

    margin-bottom:20px;

}

.cabecalho h2{

    color:#35495e;

}

.btnExcluirLista{

    background:#e74c3c;

    color:white;

    border:none;

    padding:10px 15px;

    border-radius:8px;

    cursor:pointer;

}

.btnExcluirLista:hover{

    background:#c0392b;

}

.nova{

    display:flex;

    gap:10px;

    margin-bottom:15px;

}

.nova input{

    flex:1;

    padding:12px;

    border:1px solid #ccc;

    border-radius:8px;

    font-size:15px;

}

.nova button{

    background:#42b883;

    color:white;

    border:none;

    padding:12px 20px;

    border-radius:8px;

    cursor:pointer;

}

.nova button:hover{

    background:#36996a;

}

.contador{

    margin-bottom:15px;

    color:#555;

    font-weight:bold;

}

ul{

    list-style:none;

    padding:0;

}

@media(max-width:700px){

.cabecalho{

    flex-direction:column;

    gap:15px;

}

.nova{

    flex-direction:column;

}

.nova button{

    width:100%;

}

}

</style>
