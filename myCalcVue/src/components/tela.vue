<script>
export default {
  name: "telaCalc", //nome do componente
  data() {
    return {
      //variáveis do componente
      displayTime: "",
      displayDate: "",
    };
  },
  props: {
    // variáveis passadas pelo componente pai
    output: String,
    output2: String,
  },
  mounted() {
    // parte dos Cycle Hooks que faz a hora aparecer em tempo real
    setInterval(() => {
      this.setDisplayDateTime();
    }, 1000);
  },
  methods: {
    // mostra data e hora atuais
    setDisplayDateTime() {
      this.displayDate = new Date().toLocaleDateString("pt-br", {
        day: "2-digit",
        month: "long",
        year: "numeric",
      });

      this.displayTime = new Date().toLocaleTimeString("pt-br");
    }
  },
};
</script>

<template>
  <div>
    <!-- tela para mostrar data e hora -->
    <div class="row d-flex justify-content-between">
      <div class="col">
        <p id="display-time">
          {{ displayTime }}
        </p>
      </div>
      <div class="col">
        <p id="display-date" class="d-flex justify-content-end">
          {{ displayDate }}
        </p>
      </div>
    </div>
    <div>
      <div class="row">
        <!-- tela auxiliar -->
        <p id="display-sec" class="d-flex justify-content-end display-6">
          {{ output2 || "" }}
        </p>
        <!-- tela principal -->
        <p id="display-main" class="d-flex justify-content-end display-3">
          {{ output || 0 }}
        </p>
      </div>
    </div>
  </div>
</template>
