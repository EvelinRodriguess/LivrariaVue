<script setup>
import { ref, reactive, onMounted } from "vue";
import AutorApi from "@/api/autor";
const autorApi = new AutorApi();

const defaultAutor = { id: null, descricao: "" };
const Autor = ref([]);
const autor = reactive({ ...defaultAutor });

onMounted(async () => {
  autor.value = await autor.buscarTodosAutores();
});

function limpar() {
  Object.assign(autor, { ...defaultAutor });
}

async function salvar() {
  if (autor.id) {
    await autorApi.atualizarAutor(autor);
  } else {
    await autorApi.adicionarAutor(autor);
  }
  autor.value = await autorApi.buscarTodosOsAutores();
  limpar();
}

function editar(autor_para_editar) {
  Object.assign(autor, autor_para_editar);
}

async function excluir(id) {
  await autorApi.excluirAutor(id);
  autor.value = await autorApi.buscarTodosOsAutores();
  limpar();
}
</script>

<template>
  <h1>Autores</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="autor.descricao" placeholder="Descrição" />
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="autor in autores" :key="autor.id">
      <span @click="editar(autor)">
        ({{ autor.id }}) - {{ autor.descricao }} -
      </span>
      <button @click="excluir(autor.id)">X</button>
    </li>
  </ul>
</template>

<style></style>