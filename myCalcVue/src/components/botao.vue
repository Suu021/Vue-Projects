<script>
export default {
  name: "botaoVue", // nome do componente
  props: {
    nome: String, // variável passada pelo componente pai
  },
  emits: ["funcao"], // função passada pelo componente pai
};
</script>

<template>
  <div>
    <!-- template de botão simples -->
    <button class="tecla1 btn btn-lg" @click="$emit('funcao')">
      {{ nome }}
    </button>
  </div>
</template>

<style>
/* cores para botão simples*/
.tecla1 {
  background-color: orange;
}
.tecla1:hover {
  background-color: red;
}
</style>
