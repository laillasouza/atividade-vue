<script setup lang="ts">

interface Tarefa{
    id:number
    texto:string
    concluida:boolean
}

defineProps<{
    tarefa:Tarefa
}>()

const emit = defineEmits<{
    (e:'remover',id:number):void
}>()

</script>

<template>

<li class="item">

    <div class="ladoEsquerdo">

        <input
            type="checkbox"
            v-model="tarefa.concluida"
        >

        <span
            :class="{concluida:tarefa.concluida}"
        >

            {{ tarefa.texto }}

        </span>

    </div>

    <button
        class="btnExcluir"
        @click="emit('remover', tarefa.id)"
    >

        🗑

    </button>

</li>

</template>

<style scoped>

.item{

    display:flex;

    justify-content:space-between;

    align-items:center;

    padding:12px;

    margin-bottom:10px;

    border-radius:10px;

    background:#f8f8f8;

}

.item:hover{

    background:#ececec;

}

.ladoEsquerdo{

    display:flex;

    align-items:center;

    gap:12px;

}

input{

    width:18px;

    height:18px;

    cursor:pointer;

}

span{

    font-size:16px;

}

.concluida{

    text-decoration:line-through;

    color:gray;

}

.btnExcluir{

    background:#ff5c5c;

    color:white;

    border:none;

    border-radius:8px;

    width:40px;

    height:40px;

    cursor:pointer;

    font-size:18px;

}

.btnExcluir:hover{

    background:#d63c3c;

}

</style>
