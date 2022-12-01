<!-- eslint-disable prettier/prettier -->
<script>
  //imports dos outros componentes
  import botaoSecVue from './botaoSec.vue'; 
  import botaoVue from './botao.vue';
  import telaCalc from './tela.vue'
    export default {
      name: 'calculadoraSimples', //nome do componente
      components: {
        //componentes importados
        botaoSecVue,
        botaoVue,
        telaCalc
      },
      data(){
        return {
          //variáveis do componente calculadoraSimples
          output: '',
          output2: '',
          numAnterior: null,
          operacaoSet: false,
          operacao: ''
        }
      },
      mounted() {
        // parte dos Cycle Hooks que irá iniciar o listener do teclado (Pq o @Keyup, @Keypress, @Keydown simplesmente não funcionam com botões...)
        document.addEventListener( "keyup", this.handleTecla );
      },
      methods: {
        // direciona as teclas pressionadas no teclado para as suas respectivas funções
        handleTecla(event) {
          let key = event.key;
          if (!isNaN(key)) this.getNum(key);
          else if (["+", "-", "*", "/", "e"].includes(key)) this.getOperacao(key);
          else if (key == "Enter") this.atualizarOutput();
          else if (key == "Backspace") this.apagarNum();
          else if (key == "Escape") this.limparTela();
          else if (key == ".") this.getPonto();
          else if (key == "r") this.calcularDiv1();
          else if (key == "q") this.calcularRaiz();
          else if (key == "s") this.inverterSinal();
          else if (key == "f") this.calcularFatorial();
          else if (key == "p") this.calcularPorcentagem();
        },
        // apaga os números no output
        apagarNum(){
          this.output = this.output.split('')
          this.output.pop()
          this.output = this.output.join('')
        },
        // formata o output caso a saída seja grande demais pro tamanho da tela
        formatarSaida(output){
          if(typeof output == "string") return output
          else if(output < 10){
            output = output.toString().length > 10? `${(output.toFixed(10)).toString()}` : `${output}`
            return output
          }else if(output < 100){
            output = output.toString().length > 9? `${(output.toFixed(9)).toString()}` : `${output}`
          return output
          }else if(output < 1000){
            output = output.toString().length > 8? `${(output.toFixed(8)).toString()}` : `${output}`
          return output
          }else if(output < 10000){
            output = output.toString().length > 7? `${(output.toFixed(7)).toString()}` : `${output}`
          return output
          }else if(output < 100000){
            output = output.toString().length > 6? `${(output.toFixed(6)).toString()}` : `${output}`
          return output
          }else if(output < 1000000){
            output = output.toString().length > 5? `${(output.toFixed(5)).toString()}` : `${output}`
          return output
          }else if(output < 10000000){
            output = output.toString().length > 4? `${(output.toFixed(4)).toString()}` : `${output}`
          return output
          }else{
            output = output.toString().length > 3? `${(output.toFixed(3)).toString()}` : `${output}`
          return output
          }
          
        },
        // reinicia as principais variáveis
        limparTela(){
          this.output = ''
          this.output2 = ''
          this.numAnterior = null
        },
        // inverte o sinal do output
        inverterSinal(){
          this.output = this.output[0] === '-'? this.output.slice(1) : `-${this.output}`
        },
        // faz o cálculo de porcentagem
        calcularPorcentagem(){
          this.output = this.formatarSaida(parseFloat(this.output)/100)
        },
        // faz o cálculo de raíz quadrada
        calcularRaiz(){
          this.output = this.formatarSaida(Math.sqrt(parseFloat(this.output)))
        },
        // faz o cálculo de fatorial
        calcularFatorial(){
          let p = 1
          for (let i = parseInt(this.output); i > 0; i--) p *= i
          this.output = this.formatarSaida(p)
        },
        // faz o cálculo de 1 dividido por número
        calcularDiv1(){
          this.output = this.formatarSaida(1/parseFloat(this.output))
        },
        // mostra no output os números digitados/clickados
        getNum(num){
          if(this.operacaoSet){
              this.output = ''
              this.operacaoSet = false
          }
          this.output = `${this.output}${num}`
        },
        // adiciona ponto no output
        getPonto(){
          if(this.output == '') this.output = '0.'
          if(this.output.indexOf('.') === -1) this.output = this.output+'.'
        },
        // adiciona operação à variável operacao
        getOperacao(op){
          if(this.output == '') this.output = '0'
          this.operacao = op
          this.output2 = '' 
          this.numAnterior = this.output
          this.output2 += `${this.output} ${op} `
          this.operacaoSet = true  
        },
        // faz os cálculos de soma, subtração, multiplicação, divisão e potência
        processOutput(op, a='', b=''){
            if(op == '+') return parseFloat(a) + parseFloat(b)
            else if(op == '-') return parseFloat(a) - parseFloat(b)
            else if(op == '*') return parseFloat(a) * parseFloat(b)
            else if(op == '/'){
              if(b === '0') return "impossível dividir por zero!"
              else return parseFloat(a) / parseFloat(b)
            }else if(op == 'e') return Math.pow(parseFloat(a), parseFloat(b)) // na saída vai aparecer "num e num" ao invés de "num ** num" ou "num ^ num"     
        },
        // gatilho da função processOutput ao clickar ou pressionar a tecla Enter
        atualizarOutput(){
            this.output2 += `${this.output} =`
            this.output = this.formatarSaida(this.processOutput(this.operacao, this.numAnterior, this.output))
            this.numAnterior = parseFloat(this.output)          
        }
      }
    }
</script>

<template>
  <div class="calc bg bg-dark rounded-4">
    <table class="table table-borderless">
      <caption class="caption-top text-white text-center">
        Calculadora Vue
      </caption>
      <!-- tela importada do componente filho -->
      <thead>
        <th colspan="4">
          <div class="display bg bg-white rounded p-2">
            <telaCalc :output="this.output" :output2="this.output2" />
          </div>
        </th>
      </thead>
      <!-- teclado -->
      <tbody>
        <!-- botões importados de componentes filhos -->
        <tr>
          <td><botaoSecVue @funcao="calcularRaiz" nome="&radic;" /></td>
          <td><botaoSecVue @funcao="inverterSinal" nome="-/+" /></td>
          <td><botaoVue @funcao="limparTela" nome="AC" /></td>
          <td><botaoVue @funcao="apagarNum" nome="&#9003;" /></td>
        </tr>
        <tr>
          <td><botaoSecVue @funcao="getOperacao('e')" nome="x^y" /></td>
          <td><botaoSecVue @funcao="calcularDiv1" nome="1/x" /></td>
          <td><botaoSecVue @funcao="calcularFatorial" nome="n!" /></td>
          <td><botaoSecVue @funcao="getOperacao('/')" nome="&#247;" /></td>
        </tr>
        <tr>
          <td><botaoSecVue @funcao="getNum('7')" nome="7" /></td>
          <td><botaoSecVue @funcao="getNum('8')" nome="8" /></td>
          <td><botaoSecVue @funcao="getNum('9')" nome="9" /></td>
          <td><botaoSecVue @funcao="getOperacao('*')" nome="x" /></td>
        </tr>
        <tr>
          <td><botaoSecVue @funcao="getNum('4')" nome="4" /></td>
          <td><botaoSecVue @funcao="getNum('5')" nome="5" /></td>
          <td><botaoSecVue @funcao="getNum('6')" nome="6" /></td>
          <td><botaoSecVue @funcao="getOperacao('-')" nome="-" /></td>
        </tr>
        <tr>
          <td><botaoSecVue @funcao="getNum('1')" nome="1" /></td>
          <td><botaoSecVue @funcao="getNum('2')" nome="2" /></td>
          <td><botaoSecVue @funcao="getNum('3')" nome="3" /></td>
          <td><botaoSecVue @funcao="getOperacao('+')" nome="+" /></td>
        </tr>
        <tr>
          <td><botaoSecVue @funcao="calcularPorcentagem" nome="%" /></td>
          <td><botaoSecVue @funcao="getNum('0')" nome="0" /></td>
          <td><botaoSecVue @funcao="getPonto" nome="," /></td>
          <td>
            <!-- como era o único botão de sua classe, não fiz componente pra ele -->
            <button
              class="tecla btn btn-lg btn-primary"
              @click="atualizarOutput"
            >
              =
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.calc {
  width: 400px;
}
#display-time {
  font-size: smaller;
}
#display-date {
  font-size: smaller;
}
button {
  width: 70px;
}
</style>
