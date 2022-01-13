<template>
  <div class="container">
    <div>
      <input type="file" name="file" @change="tratarUpload($event)" />
      <button @click="enviarArquivo()">Enviar</button>
    </div>
    <div v-if="dadosExibicao">
      <table>
        <thead>
          <tr>
            <th>Código</th>
            <th>Nome</th>
            <th>Idade</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="dado in dadosExibicao" :key="dado[0]">
            <td>{{ dado[0]}} </td>
            <td>{{ dado[1]}} </td>
            <td>{{ dado[2]}} </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'HelloWorld',
  data() {
    return {
      //Setar aqui a url do backend
      url: 'http://127.0.0.1:8000/api/arquivo/',
      arquivo: null,
      //Enquanto dadosExibicao é null, a tabela não é exibida
      dadosExibicao: null
    }
  },

  methods: {
    // Ao selecionar um arquivo, o mesmo é setado na variável arquivo
    tratarUpload(event){
      this.arquivo = event.target.files[0];
    },
    // Aqui o envio do arquivo é feito, desde que um arquivo tenha sido selecionado
    async enviarArquivo(){
      if(this.arquivo){
        //Criar um FormData para envio do arquivo
        let formData  = new FormData();
        formData.append('arquivo', this.arquivo)
        
        //Chamada da api usando o axios
        await axios.post(this.url, formData)       
          //Se deu tudo certo, os dados são colocados na variável dadosExibicao e a tabela é mostrada   
          .then(response => {
            this.dadosExibicao = response.data.valores
          })
          //Se deu algum erro é exibido um alerta
          .catch(e => {
            alert('Verifique se o arquivo foi selecionado e está correto')
              this.errors.push(e)
            });
      }
      else
        alert("Selecione um arquivo csv para continuar");
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  tbody tr:nth-child(odd){
    background-color: #E0E0E0;
  }
  table {
      border:none;
      border-collapse: collapse;
      width: -moz-fit-content;
      width: fit-content;
      margin: auto;
      margin-top: 20px;
  }

  table td {
    border-left: 1px solid #000;
    border-right: 1px solid #000;
  }

  th, td {
    padding-left: 25px;
    padding-right: 25px
  }

  table td:first-child {
      border-left: none;
  }

  table td:last-child {
      border-right: none;
  }
  .container{
    align-items: center;
  }
</style>
