<template>
  <div class="contentAtestado" contenteditable="true">
    <div>
      <p align="center"></p>
      <p align="center">
        <font> <h5>DECLARAÇÃO DE COMPARECIMENTO</h5></font>
      </p>
    </div>

    <div
      class="teste d-flex flex-column align-items-center justify-content-center"
    >
      <div class="teste2">
        <font
          >Declaro, para os devidos fins que o paciente
          ________________________________________________, inscrito no RG nº
        </font>
      </div>
      <div class="margin">
        <font>_______________________________ esteve na clinica</font>
      </div>
      <div class="margin2"> 
        <font>
          <p><input class="text-center"  type="text" v-model="dadosEmpresa.nomeClinica" value="Nome da Empresa"/><br />
            </p>                  
        </font>
      </div>
      <div class="margin3">
        <font>no
          período de  ____  h a  ____  h,</font>
      </div>
      <div class="margin4"> 
        <font> para realizar avaliação visual.</font>
      </div>
      <div
        class="
          teste
          d-flex
          flex-column
          align-items-center
          justify-content-center
        "
      >
        <div class="teste3">
          <br />
          Local: ______ de _______________ de ______.<br />
        </div>
        <div class="margin5"> <h5><font class="teste d-flex flex-column align-items-center justify-content-center">Assinatura e Carimbo.</font></h5></div>       
      </div>
    </div>
  </div>
</template>

<script>
import ClinicaService from "../../services/clinica";

export default {
  data() {
    return {
      dadosEmpresa: [],
    };
  },
  created() {
    this.read();
  },
  methods: {
    async read() {
      try {
        const result = await ClinicaService.read();
        if (result.data.result.length === 0) {
          return;
        }
        this.dadosEmpresa = result.data.result[0];
    console.log(this.dadosEmpresa,"cha cha")

      } catch (error) {
        this.showAlert("error", "Um erro ocorreu ao ler os dados da Cliníca");
      }
    },
  },
};
</script>

<style>
.margin {
  margin-right: 400px;
  margin-top: 20px;
}

.margin2 {
  margin-top: -30px;
  margin-left: 120px;
}

.margin3 {
  margin-left: 520px;
  margin-top: -35px;
}

.margin4 {
  margin-right: 500px;
  margin-top: 25px;
}

.margin5 {
  margin-top: 200px;
}

.teste {
  height: 70%;
}
.teste2 {
  margin-top: 100px;
}

.teste3 {
  margin-top: 400px;
  margin-right: 440px;
}
.contentAtestado {
  height: 100vh;
}
</style>