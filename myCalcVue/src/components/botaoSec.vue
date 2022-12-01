<script>
export default {
  name: "botaoSecVue", // nome do componente
  props: {
    nome: String, // variável passada pelo componente pai
  },
  emits: ["funcao"], // função passada pelo componente pai
};
</script>

<template>
  <div>
    <!-- template do botão de classe btn-secundary -->
    <button class="tecla btn btn-lg btn-secondary" @click="$emit('funcao')">
      {{ nome }}
    </button>
  </div>
</template>
