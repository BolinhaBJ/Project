<template>
  <div>
    <Sidebar />
    <b-container fluid class="consulta">
      <div class="mb-5">
        <b-card no-body>
          <b-tabs card v-model="tabIndexConsulta">
            <b-tab title="Agendados para Hoje">
              <b-card class="agendadosHoje">
                <table class="table table-sm">
                  <thead>
                    <tr>
                      <th scope="col">Atendido</th>
                      <th scope="col">Nome Paciente</th>
                      <th scope="col">Data do Atendimento</th>
                      <th scope="col">Horário do Atendimento</th>
                      <th scope="col">Iniciar Consulta</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="agendamento in this.agendamentos"
                      :key="agendamento.uuid"
                    >
                      <td v-if="agendamento.atendido === 1">
                        <b-icon-check2
                          class="h3"
                          variant="success"
                        ></b-icon-check2>
                      </td>
                      <td v-else>
                        <b-icon-x class="h3"></b-icon-x>
                      </td>
                      <td>{{ agendamento.nomePaciente }}</td>
                      <td>{{ agendamento.data }}</td>
                      <td>{{ agendamento.horario }}</td>
                      <td>
                        <b-button
                          variant="warning"
                          pill
                          size="sm"
                          @click="
                            inicioConsulta(
                              agendamento.uuidPaciente,
                              agendamento.uuid,
                              agendamento.pacienteUuid,
                              agendamento.atendido
                            )
                          "
                        >
                          Consultar Paciente
                        </b-button>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <div class="mt-2 mb-2">
                  <div>
                    <b-button
                      pill
                      variant="primary"
                      class="mr-2"
                      @click="proximaPageAgHoje"
                      v-if="page < totalPage"
                      size="sm"
                      >Proxima Pagina</b-button
                    >
                    <b-button
                      pill
                      variant="primary"
                      class="mr-2"
                      disabled
                      size="sm"
                      v-else
                      >Proxima Pagina</b-button
                    >
                    <label>
                      Total de Páginas
                      <b-badge variant="primary">{{ totalPage }}</b-badge>
                    </label>
                    <label class="ml-4">
                      Página Atual
                      <b-badge variant="primary">{{ page }}</b-badge>
                    </label>
                    <b-button
                      pill
                      variant="primary"
                      @click="anteriorPageAgHoje"
                      v-if="page > 1"
                      class="mr-2 ml-2"
                      size="sm"
                      >Pagina Anterior</b-button
                    >
                    <b-button
                      pill
                      variant="primary"
                      disabled
                      v-else
                      size="sm"
                      class="mr-2 ml-2"
                      >Pagina Anterior</b-button
                    >
                  </div>
                </div>
              </b-card>
            </b-tab>

            <b-tab title="Pesquisar Agendamentos">
              <b-card class="agendadosHoje">
                <div class="flex mb-4">
                  <div class="mr-3">
                    <label>Data Inicial</label>
                    <input
                      type="date"
                      size="sm"
                      class="form-control col-sm-12"
                      v-model="dataInicialAg"
                    />
                  </div>
                  <div>
                    <label>Data Final</label>
                    <input
                      type="date"
                      class="form-control col-sm-12"
                      v-model="dataFinalAg"
                    />
                  </div>

                  <div class="ml-2">
                    <b-button @click="listAgendamento" block variant="primary"
                      >Pesquisar</b-button
                    >
                  </div>
                </div>
                <table class="table table-sm">
                  <thead>
                    <tr>
                      <th scope="col">Nome Paciente</th>
                      <th scope="col">Data do Atendimento</th>
                      <th scope="col">Horário do Atendimento</th>
                      <th scope="col">Iniciar Consulta</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="agendamento in this.agendamentosData"
                      :key="agendamento.uuid"
                    >
                      <td>{{ agendamento.nomePaciente }}</td>
                      <td>{{ agendamento.data }}</td>
                      <td>{{ agendamento.horario }}</td>
                      <td>
                        <b-button
                          variant="warning"
                          pill
                          size="sm"
                          @click="
                            inicioConsulta(
                              agendamento.pacienteUuid,
                              agendamento.uuid,
                              agendamento.pacienteUuid,
                              agendamento.procedimento
                            )
                          "
                        >
                          Consultar Paciente
                        </b-button>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <div class="mt-2 mb-2">
                  <div>
                    <b-button
                      pill
                      variant="primary"
                      class="mr-2"
                      @click="proximaPageAg"
                      v-if="pageAg < totalPageAg"
                      size="sm"
                      >Proxima Pagina</b-button
                    >
                    <b-button
                      pill
                      variant="primary"
                      class="mr-2"
                      disabled
                      size="sm"
                      v-else
                      >Proxima Pagina</b-button
                    >
                    <label>
                      Total de Páginas
                      <b-badge variant="primary">{{ totalPageAg }}</b-badge>
                    </label>
                    <label class="ml-4">
                      Página Atual
                      <b-badge variant="primary">{{ pageAg }}</b-badge>
                    </label>
                    <b-button
                      pill
                      variant="primary"
                      @click="anteriorPageAg"
                      v-if="pageAg > 1"
                      class="mr-2 ml-2"
                      size="sm"
                      >Pagina Anterior</b-button
                    >
                    <b-button
                      pill
                      variant="primary"
                      disabled
                      v-else
                      size="sm"
                      class="mr-2 ml-2"
                      >Pagina Anterior</b-button
                    >
                  </div>
                </div>
              </b-card>
            </b-tab>

            <b-tab title="Consulta">
              <h3 class="text-center mt-3">Consulta Optometria</h3>
            </b-tab>
          </b-tabs>
        </b-card>
      </div>
      <b-card-group
        v-if="tabIndexConsulta === 2"
        deck
        style="display: flex; justify-content: center; flex-wrap: wrap"
        class="mb-4"
      >
        <b-card
          bg-variant="light"
          title="Nome Paciente"
          class="text-center col-sm-3"
        >
          <b-avatar class="mr-4"></b-avatar>
          <span class="mr-auto">{{ nomePaciente }}</span>
        </b-card>

        <b-card
          bg-variant="light"
          title="Data Nascimento"
          class="text-center col-sm-3"
        >
          <b-icon-calendar2-date class="h3"></b-icon-calendar2-date>
          <b-card-text>{{ dataPaciente }}</b-card-text>
        </b-card>

        <b-card
          bg-variant="light"
          title="Telefone de Contato"
          class="text-center col-sm-3"
        >
          <img
            :src="imgWhats"
            @click="enviarmsg"
            v-b-popover.hover="
              'Clique para ser redirecinado ao Whatsapp do Paciente'
            "
            class="mb-1 whatsPaciente"
            alt=""
            srcset=""
          />
          <b-card-text>{{ telefonePaciente }}</b-card-text>
        </b-card>
      </b-card-group>

      <div v-if="tabIndexConsulta === 2">
        <b-card no-body>
          <b-tabs v-model="tabIndex" card>
            <b-tab title="Prescrição para Óculos" active>
              <b-card class="prescricao">
                <PrescricaoOculos :PrescricaoProps="this.prescricaoOculos" />
              </b-card>
            </b-tab>
            <b-tab title="Prescrição para Lente de Contato">
              <b-card class="prescricao">
                <PrescricaoLente :PrescricaoLenteProps="this.prescricaoLente" />
              </b-card>
            </b-tab>
            <b-tab title="Laudo">
              <Laudo />
            </b-tab>
            <b-tab title="Atestado">
              <b-card class="prescricao">
                <div class="row d-flex justify-content-center">
                  <b-dropdown
                    style="width: 350px; float:"
                    variant="primary"
                    id="dropdown-1"
                    text="Atestado"
                    class="m-2"
                  >
                    <b-dropdown-item style="width: 350px; float:" @click="imprimirAtestado()">Modelo 1</b-dropdown-item>
                    
                    <b-dropdown-item style="width: 350px; float:" @click="imprimirAtestado2()">Modelo 2</b-dropdown-item>                 
                  </b-dropdown>
                </div>
              </b-card>
            </b-tab>
            <b-tab title="Declaração">
              <b-card no-body class="mb-5">
                <b-card-header header-tag="header" class="p-1" role="tab">
                  <b-button
                    block
                    v-b-toggle.accordionPesquisaPrescriOculos
                    variant="info"
                    >Pesquisar Declaração</b-button
                  >
                </b-card-header>
                <b-collapse
                  id="accordionPesquisaPrescriOculos"
                  visible
                  accordion="my-accordion"
                  role="tabpanel"
                >
                  <b-card-body>
                    <div>
                      <label for="">Selecione o Paciente</label>
                      <b-form-select
                        v-model="pacienteSelectedDeclaracao"
                        :options="ListaPaciente"
                        size="sm"
                        class="mb-3"
                        value-field="value"
                        text-field="text"
                        disabled-field="notEnabled"
                      >
                        <template #first>
                          <b-form-select-option :value="null"
                            >-- Paciente --</b-form-select-option
                          >
                        </template>
                      </b-form-select>
                      <div>
                        <div style="width: 100%">
                          <div style="width: 100%">
                            <label for="example-datepicker">Data Inicial</label>
                            <b-form-datepicker
                              id="example-datepicker"
                              size="sm"
                              v-model="dataInicial"
                              class="mb-2 col-sm-3"
                              placeholder="Data não Informada"
                            ></b-form-datepicker>
                          </div>

                          <div>
                            <label for="example-datepicker">Data Final</label>
                            <b-form-datepicker
                              id="example-datepicker"
                              size="sm"
                              v-model="dataFinal"
                              class="mb-2 col-sm-3"
                              placeholder="Data não Informada"
                            ></b-form-datepicker>
                          </div>
                          <b-button
                            variant="primary"
                            size="sm"
                            class="mb-4 mt-2"
                            >Pesquisar</b-button
                          >
                        </div>
                      </div>
                    </div>
                    <table class="table table-sm">
                      <thead>
                        <tr>
                          <th scope="col">Nome Paciente</th>
                          <th scope="col">Data Consulta</th>
                          <th scope="col">Visualizar</th>
                          <th scope="col">Imprimir</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="prescri in this.prescricoes"
                          :key="prescri.uuid"
                        >
                          <th>{{ prescri.nomePaciente }}</th>
                          <td>{{ prescri.data }}</td>
                          <td>
                            <b-button
                              variant="primary"
                              size="sm"
                              @click="readPrescricaoUuid(prescri.uuid)"
                            >
                              Visualizar
                            </b-button>
                          </td>
                          <td>
                            <b-button
                              variant="primary"
                              size="sm"
                              @click="imprimirPrescri(prescri.uuid)"
                            >
                              Imprimir
                            </b-button>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </b-card-body>
                </b-collapse>
              </b-card>
              <b-card class="prescricao">
                <Editor />
              </b-card>
            </b-tab>
            <b-tab title="Encaminhamento">
              <b-card no-body class="mb-5">
                <b-card-header header-tag="header" class="p-1" role="tab">
                  <b-button
                    block
                    v-b-toggle.accordionPesquisaPrescriOculos
                    variant="info"
                    >Pesquisar Encaminhamento</b-button
                  >
                </b-card-header>
                <b-collapse
                  id="accordionPesquisaPrescriOculos"
                  visible
                  accordion="my-accordion"
                  role="tabpanel"
                >
                  <b-card-body>
                    <div>
                      <label for="">Selecione o Paciente</label>
                      <b-form-select
                        v-model="pacienteSelectedEncaminhamento"
                        :options="ListaPaciente"
                        size="sm"
                        class="mb-3"
                        value-field="value"
                        text-field="text"
                        disabled-field="notEnabled"
                      >
                        <template #first>
                          <b-form-select-option :value="null"
                            >-- Paciente --</b-form-select-option
                          >
                        </template>
                      </b-form-select>
                      <div>
                        <div style="width: 100%">
                          <div style="width: 100%">
                            <label for="example-datepicker">Data Inicial</label>
                            <b-form-datepicker
                              id="example-datepicker"
                              size="sm"
                              v-model="dataInicial"
                              class="mb-2 col-sm-3"
                              placeholder="Data não Informada"
                            ></b-form-datepicker>
                          </div>

                          <div>
                            <label for="example-datepicker">Data Final</label>
                            <b-form-datepicker
                              id="example-datepicker"
                              size="sm"
                              v-model="dataFinal"
                              class="mb-2 col-sm-3"
                              placeholder="Data não Informada"
                            ></b-form-datepicker>
                          </div>
                          <b-button
                            variant="primary"
                            size="sm"
                            class="mb-4 mt-2"
                            >Pesquisar</b-button
                          >
                        </div>
                      </div>
                    </div>
                    <table class="table table-sm" id="myTable">
                      <thead>
                        <tr>
                          <th scope="col">Nome Paciente</th>
                          <th scope="col">Data Consulta</th>
                          <th scope="col">Visualizar</th>
                          <th scope="col">Imprimir</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr
                          v-for="prescri in this.prescricoes"
                          :key="prescri.uuid"
                        >
                          <th>{{ prescri.nomePaciente }}</th>
                          <td>{{ prescri.data }}</td>
                          <td>
                            <b-button
                              variant="primary"
                              size="sm"
                              @click="readPrescricaoUuid(prescri.uuid)"
                            >
                              Visualizar
                            </b-button>
                          </td>
                          <td>
                            <b-button
                              variant="primary"
                              size="sm"
                              @click="imprimirPrescri(prescri.uuid)"
                            >
                              Imprimir
                            </b-button>
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </b-card-body>
                </b-collapse>
              </b-card>
              <b-card class="prescricao">
                <Editor />
              </b-card>
            </b-tab>
            <b-tab title="Ficha Clínica">
              <b-card no-body class="mb-5">
                <!-- <b-card-header header-tag="header" class="p-1" role="tab">
                  <b-button
                    block
                    v-b-toggle.accordionPesquisaPrescriOculos
                    variant="transparent"
                    class="shadow"
                    >Pesquisar Ficha Clínica
                    <b-icon-search class="ml-2"></b-icon-search
                  ></b-button>
                </b-card-header> -->
                <!-- <b-collapse
                  id="accordionPesquisaPrescriOculos"
                  visible
                  accordion="my-accordion"
                  role="tabpanel"
                  aria-expanded="true"
                > -->
                <b-card-body>
                  <div>
                    <label for="">Selecione o Paciente</label>
                    <b-form-select
                      v-model="pacienteSelectedFicha"
                      :options="ListaPaciente"
                      size="sm"
                      class="mb-3"
                      value-field="value"
                      text-field="text"
                      disabled-field="notEnabled"
                    >
                      <template #first>
                        <b-form-select-option :value="null"
                          >-- Paciente --</b-form-select-option
                        >
                      </template>
                    </b-form-select>
                    <div>
                      <div style="width: 100%">
                        <div style="width: 100%">
                          <label for="example-datepicker">Data Inicial</label>
                          <b-form-datepicker
                            id="example-datepicker"
                            size="sm"
                            v-model="dataInicalFicha"
                            class="mb-2 col-sm-3"
                            placeholder="Data não Informada"
                          ></b-form-datepicker>
                        </div>

                        <div>
                          <label for="example-datepicker">Data Final</label>
                          <b-form-datepicker
                            id="example-datepicker"
                            size="sm"
                            v-model="dataFinalFicha"
                            class="mb-2 col-sm-3"
                            placeholder="Data não Informada"
                          ></b-form-datepicker>
                        </div>
                        <b-button
                          variant="primary"
                          size="sm"
                          class="mb-4 mt-2"
                          @click="pesquisarFicha"
                          >Pesquisar</b-button
                        >
                        <router-link
                          class="nav-link mr-5"
                          to="/Impressao"
                          style="padding: 0"
                        >
                          <b-button
                            variant="primary"
                            size="sm"
                            class="mb-3 ml-2"
                            >Imprimir Ficha em Branco</b-button
                          >
                        </router-link>
                      </div>
                    </div>
                  </div>
                  <table class="table table-sm" id="teste">
                    <thead>
                      <tr>
                        <th scope="col">Nome Paciente</th>
                        <th scope="col">Data Consulta</th>
                        <th scope="col">Visualizar</th>
                        <th scope="col">Excluir</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="ficha in this.fichaPesquisa" :key="ficha.uuid">
                        <th>{{ ficha.nomePaciente }}</th>
                        <td>{{ ficha.data }}</td>
                        <td>
                          <b-button
                            variant="primary"
                            size="sm"
                            @click="visualizar(ficha.uuid)"
                          >
                            Visualizar
                          </b-button>
                        </td>
                        <!-- <td>
                            <b-button
                              variant="primary"
                              size="sm"
                              @click="imprimirPrescri(ficha.uuid)"
                            >
                              Imprimir
                            </b-button>
                          </td> -->

                        <td>
                          <b-button
                            variant="primary"
                            size="sm"
                            @click="deleteFicha(ficha.uuid)"
                          >
                            Excluir
                          </b-button>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </b-card-body>
                <!-- </b-collapse> -->
              </b-card>
              <b-card class="fichaClinica">
                <Accordion
                  :fichaClinicaProps="this.fichaClinica"
                  :Visualizar="this.VisualizarBool"
                  :uuidFichaProps="this.uuidFicha"
                  :editarProps="this.editar"
                  :iniciarConsultaProps="this.iniciarConsulta"
                  @mudarEditar="mudarEditar"
                  @finalizado="limparUser"
                  @ConsultaFinalizada="loadAgendamentos"
                />
              </b-card>
            </b-tab>
          </b-tabs>
        </b-card>
      </div>
    </b-container>
  </div>
</template>

<script>
import Accordion from "./Accordion";
import PrescricaoOculos from "./PrescricaoOculos";
import PrescricaoLente from "./PrescricaoLente";
//import Atestado from "./Atestado";
import Editor from "../editor_text/editor";
import { mapActions, mapState } from "vuex";
import FichaClinicaService from "../../services/fichaClinica";
import serviceClinica from "../../services/clinica";
import PrescricaoOculosService from "../../services/prescricaoOculos";
import PacienteService from "../../services/paciente";
import PrescricaoLenteService from "../../services/prescicaoLente";
import Pesquisa from "../../services/pesquisaConsulta";
import AgendaService from "../../services/agenda";
import ServicoConsulta from "../../services/consulta";
import Laudo from "../Consulta/Laudo/LaudoPage";
import LaudoService from "../../services/laudo";
import imgWhats from "../../assets/whatsapp.png";
import Sidebar from "../../components/SidebarNavbar.vue";
import jsPDF from "jspdf";
import "jspdf-autotable";

//import { DateTime } from "luxon";
import moment from "moment";

export default {
  components: {
    Accordion,
    //Atestado,
    PrescricaoOculos,
    PrescricaoLente,
    Editor,
    Laudo,
    Sidebar,
  },
  updated() {},

  data() {
    return {
      imgWhats: imgWhats,
      prescricoes: [],
      pacienteSelectedDeclaracao: null,
      pacienteSelectedFicha: null,
      pacienteSelectedAtestado: null,
      pacienteSelectedLaudo: null,
      pacienteSelectedEncaminhamento: null,
      dataInicalFicha: "",
      dataFinalFicha: "",
      pacientes: [],
      fichaPesquisa: [],
      iniciarConsulta: false,
      editar: false,
      uuidFicha: "",
      nomePaciente: "",
      dataPaciente: "",
      telefonePaciente: "",
      abreviaNome: "",
      selectVisualizar: "",
      color: "primary",
      dataInicialAg: "",
      dataFinalAg: "",
      pagePesquisa: 1,
      totalPagePesquisa: 1,
      dataInicialAgFinalizado: "",
      dataFinalAgFinalizado: "",
      agendamentos: [],
      procedimentoConsulta: "",
      agendamentosDataFinalizado: [],
      agendamentosData: [],
      select: null,
      prescricaoOculos: {},
      prescricaoLente: {},
      fichaClinica: {},
      selectPesquisa: null,
      tabIndexConsulta: 0,
      ListaPaciente: [],
      ListaConsulta: {},
      typePesquisa: {},
      page: 1,
      totalPage: 1,
      pageAg: 1,
      totalPageAg: 1,
      pageFinalizado: 1,
      totalPageFinalizado: 1,
      dadosConsulta: {
        idPaciente: 0,
        data: "",
        titulo: "Laudo",
      },
      Options: [
        { text: "Prescrição Lente", value: "prescricao_lente" },
        { text: "Prescrição Óculos", value: "prescricao_oculos" },
        { text: "Laudo", value: "laudo" },
        { text: "Atestado", value: "atestado" },
        { text: "Encaminhamento", value: "encaminhamento" },
        { text: "Ficha Clínica", value: "ficha_clinica" },
      ],
      VisualizarConsulta: [],
      VisualizarBool: false,
      tabIndex: 0,
      dataInicial: "",
      dataFinal: "",
      uuidPaciente: "",
      laudo: {
        uuid: "",
        idConsulta: "",
        data: moment().format("YYYY-MM-DD"),
        idPaciente: "",
        od_perto_sc: "",
        od_perto_cc: "",
        od_longe_sc: "",
        od_longe_cc: "",
        oe_perto_sc: "",
        oe_perto_cc: "",
        oe_longe_sc: "",
        oe_longe_cc: "",
      },
      options: [
        { text: "Orange", value: "orange" },
        { text: "Apple", value: "apple" },
        { text: "Pineapple", value: "pineapple" },
        { text: "Grape", value: "grape" },
        { text: "Apple", value: "apple" },
        { text: "Pineapple", value: "pineapple" },
        { text: "Grape", value: "grape" },
        { text: "Pineapple", value: "pineapple" },
        { text: "Grape", value: "grape" },
        { text: "Apple", value: "apple" },
        { text: "Pineapple", value: "pineapple" },
        { text: "Grape", value: "grape" },
        { text: "Grape", value: "grape" },
      ],
    };
  },
  computed: {
    ...mapState({
      anamnese: (state) => state.anamnese,
      idPaciente: (state) => state.pacienteSelected,
    }),
  },
  methods: {
    imprimirPrescri() {
      const doc = new jsPDF();

      // It can parse html:
      // <table id="my-table"><!-- ... --></table>

      doc.autoTable({
        html: "#teste",
        margin: { horizontal: 5 },
        startY: 20,
        styles: { fontSize: 7 },
        tableWidth: 220,
        tableId: "salesTable",
      });

      //  doc.autoTable({
      //       html: "#myTable",
      //     margin: { horizontal: 160 },
      //     styles: { fontSize: 12 },
      //     startY: 20,
      //     tableWidth: 100,
      //   });

      // Or use javascript directly:
      //      doc.autoTable({
      //   columnStyles: { europe: { halign: 'center' } }, // European countries centered
      //   body: [
      //     { europe: 'Sweden', america: 'Canada', asia: 'China' },
      //     { europe: 'Norway', america: 'Mexico', asia: 'Japan' },
      //   ],
      //   columns: [
      //     { header: 'Europe', dataKey: 'europe' },
      //     { header: 'Asia', dataKey: 'asia' },
      //   ],
      // })

      window.open(doc.output("bloburl"));
    },
    ...mapActions(["pacienteSelected"]),

    mudarEditar() {
      this.editar = false;
    },

    async readClinica() {
      try {
        const clinica = await serviceClinica.read();
        this.$store.commit("dadosClinica", clinica.data.result[0]);
      } catch (error) {
        console.log(error);
        this.showAlert("error", "Ocorreu um erro ao listar dados clinica");
      }
    },

    imprimirAtestado() {
      if (this.uuidPaciente) {
        window.open(
          `/Impressao/atestado/${this.uuidPaciente}`,
          "_blank",
          "toolbar=yes, scrollbars=yes, resizable=yes, top=200, left=200, width=1000, height=1000"
        );
      } else {
        this.showAlert("info", "Por favor selecione o Paciente");
      }
    },

    imprimirAtestado2() {
      if (this.uuidPaciente) {
        window.open(
          `/Impressao/atestado2/${this.uuidPaciente}`,
          "_blank",
          "toolbar=yes, scrollbars=yes, resizable=yes, top=200, left=200, width=1000, height=1000"
        );
      } else {
        this.showAlert("info", "Por favor selecione o Paciente");
      }
    },

    gerarLaudo() {
      if (this.uuidPaciente) {
        window.open(
          `/Impressao/laudo/${this.uuidPaciente}`,
          "_blank",
          "toolbar=yes, scrollbars=yes, resizable=yes, top=200, left=200, width=1000, height=1000"
        );
      } else {
        this.showAlert("info", "Por favor selecione o Paciente");
      }
    },

    anteriorPageAg() {
      this.pageAg = this.pageAg - 1;
      AgendaService.readDateRelatorioPaginationNavigation(
        this.dataInicialAg,
        this.dataFinalAg,
        this.pageAg
      ).then((result) => {
        this.agendamentosData = result.data.agendamentos.result;
        this.agendamentosData.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
        this.agendamentos.sort(this.compararHora);
      });
    },

    proximaPageAg() {
      this.pageAg = this.pageAg + 1;
      AgendaService.readDateRelatorioPaginationNavigation(
        this.dataInicialAg,
        this.dataFinalAg,
        this.pageAg
      ).then((result) => {
        this.agendamentosData = result.data.agendamentos.result;
        this.agendamentosData.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
        this.agendamentos.sort(this.compararHora);
      });
    },

    proximaPageAgHoje() {
      this.page = this.page + 1;
      AgendaService.readDateInnerPaginationNavigation(
        moment().add("day", 1).format("YYYY-MM-DD"),
        this.page
      ).then((result) => {
        this.agendamentos = result.data.agendamentos.result;
        this.agendamentos.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
        this.agendamentos.sort(this.compararHora);
      });
    },

    proximaPageFinalizado() {
      this.pageFinalizado = this.pageFinalizado + 1;
      this.agendamentosDataFinalizado = [];
      AgendaService.readDateAgendamentoFinalizadoPaginationNavigation(
        this.dataInicialAgFinalizado,
        this.dataFinalAgFinalizado,
        this.pageFinalizado
      ).then((result) => {
        this.agendamentosDataFinalizado = result.data.agendamentos.result;
        this.agendamentosDataFinalizado.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
      });
    },

    anteriorPageFinalizado() {
      this.pageFinalizado = this.pageFinalizado - 1;
      this.agendamentosDataFinalizado = [];
      AgendaService.readDateAgendamentoFinalizadoPaginationNavigation(
        this.dataInicialAgFinalizado,
        this.dataFinalAgFinalizado,
        this.pageFinalizado
      ).then((result) => {
        this.agendamentosDataFinalizado = result.data.agendamentos.result;
        this.agendamentosDataFinalizado.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
      });
    },

    anteriorPageAgHoje() {
      this.page = this.page - 1;
      AgendaService.readDateInnerPaginationNavigation(
        moment().format("YYYY-MM-DD"),
        this.page
      ).then((result) => {
        this.agendamentos = result.data.agendamentos.result;
        this.agendamentos.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
        this.agendamentos.sort(this.compararHora);
      });
    },

    listAgendamento() {
      AgendaService.readDateRelatorioPagination(
        this.dataInicialAg,
        this.dataFinalAg
      ).then((result) => {
        this.agendamentosData = result.data.agendamentos.result;
        this.totalPageAg = Math.ceil(
          result.data.agendamentos.total[0].count / 5
        );
        this.agendamentosData.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
        if (this.dataInicialAg === this.dataFinalAg) {
          this.agendamentosData.sort(this.compararHora);
        } else {
          this.agendamentosData.sort(this.compararData);
        }
      });
    },

    listAgendamentoRealizado() {
      this.totalPageFinalizado = 1;
      this.pageFinalizado = 1;
      this.agendamentosDataFinalizado = [];
      AgendaService.readDateAgendamentoFinalizadoPagination(
        this.dataInicialAgFinalizado,
        this.dataFinalAgFinalizado
      ).then((result) => {
        this.agendamentosDataFinalizado = result.data.agendamentos.result;
        this.totalPageFinalizado = Math.ceil(
          result.data.agendamentos.total[0].count / 5
        );
        this.agendamentosDataFinalizado.map((el) => {
          el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
        });
        if (this.dataInicialAgFinalizado === this.dataFinalAgFinalizado) {
          this.agendamentosDataFinalizado.sort(this.compararHora);
        } else {
          this.agendamentosDataFinalizado.sort(this.compararData);
        }
      });
    },

    inicioConsulta(idPaciente, uuidAgendamento, pacienteUuid, atendido) {
      if (atendido === 1) {
        this.showAlert("info", "Paciente já foi Atendido");
      } else {
        this.$store.commit("PACIENTE_SELECTED", idPaciente);
        this.$store.commit("UUID_AGENDAMENTO", uuidAgendamento);
        PacienteService.read(pacienteUuid).then((result) => {
          this.uuidPaciente = pacienteUuid;
          this.nomePaciente = result.data.paciente.nomePaciente;
          this.telefonePaciente = result.data.paciente.telefone;
          this.dataPaciente = moment(result.data.paciente.dataNascimento)
            .add("day", 1)
            .format("DD/MM/YYYY");
          this.abreviaNome = this.nomePaciente[0];
        });
        this.tabIndexConsulta = 2;
        this.showAlert("success", "Pronto para Iniciar consulta");
      }
    },

    compararHora(a, b) {
      console.table("entroo");
      return parseInt(a.horario) - parseInt(b.horario);
    },

    compararData(a, b) {
      console.table("entroo");
      return parseInt(a.data) - parseInt(b.data);
    },

    loadAgendamentos() {
      this.page = 1;
      this.totalPage = 1;
      AgendaService.readDateInnerPagination(moment().format("YYYY-MM-DD")).then(
        (result) => {
          this.agendamentos = result.data.agendamentos.result;
          this.totalPage = Math.ceil(
            result.data.agendamentos.total[0].count / 5
          );
          this.agendamentos.map((el) => {
            console.log(el.data);
            el.data = moment(el.data).add("day", 1).format("DD/MM/YYYY");
          });
        }
      );
    },

    showAlert(icon, title) {
      // Use sweetalert2
      this.$swal({
        icon: icon,
        title: title,
        showConfirmButton: false,
        timer: 2500,
      });
    },

    alteraProcedimento() {
      this.ListaConsulta = {};
    },

    async deleteFicha(uuid) {
      try {
        await FichaClinicaService.delete(uuid);
        await this.pesquisarFicha();
        this.showAlert("success", "Registro Deletado com Sucesso");
      } catch (error) {
        console.log(error);
        this.showAlert("error", "Ocorreu um erro ao deletar Registro");
      }
    },

    async pesquisarFicha() {
      try {
        this.fichaPesquisa = [];
        if (this.pacienteSelectedFicha === null) {
          const ficha = await FichaClinicaService.readDate(
            this.dataInicalFicha,
            this.dataFinalFicha
          );
          ficha.data.result.forEach((element) => {
            element.data = moment(element.data)
              .add("day", 1)
              .format("DD/MM/YYYY");
            this.fichaPesquisa.push(element);
          });

          return;
        }
        const ficha = await FichaClinicaService.readPagination(
          this.pacienteSelectedFicha,
          this.dataInicalFicha,
          this.dataFinalFicha
        );
        ficha.data.result.result.forEach((element) => {
          element.data = moment(element.data)
            .add("day", 1)
            .format("DD/MM/YYYY");
          this.fichaPesquisa.push(element);
        });
      } catch (error) {
        this.showAlert("error", "Ocorreu um erro ao pesquisar Ficha");
      }
    },

    // retornaTipoConsulta() {
    //   if (this.typePesquisa === "prescricao_lente") {
    //     return "Prescrição Lente";
    //   } else if (this.typePesquisa === "prescricao_oculos") {
    //     return "Prescrição Óculos";
    //   } else if (this.typePesquisa === "ficha_clinica") {
    //     return "Ficha Clínica";
    //   } else if (this.typePesquisa === "laudo") {
    //     return "Laudo";
    //   } else if (this.typePesquisa === "atestado") {
    //     return "Atestado";
    //   } else if (this.typePesquisa === "encaminhamento") {
    //     return "Encaminhamento";
    //   }
    // },

    limparUser() {
      this.nomePaciente = "";
      this.dataPaciente = "";
      this.abreviaNome = "";
      this.procedimentoConsulta = "";
      this.$store.commit("PACIENTE_SELECTED", -1);
      this.$store.commit("UUID_AGENDAMENTO", "");
    },

    // atualizaPesquisa() {
    //   Pesquisa(this.typePesquisa, {
    //     idPaciente: this.idPaciente,
    //     dataInicial: this.dataInicial,
    //     dataFinal: this.dataFinal,
    //   }).then((result) => {
    //     this.ListaConsulta = result.data.result;
    //     this.ListaConsulta.titulo = this.retornaTipoConsulta();
    //   });
    // },

    proximaPagePesquisa() {
      this.pagePesquisa = this.pagePesquisa + 1;

      FichaClinicaService.readPagination(
        this.idPaciente,
        this.dataInicial,
        this.dataFinal,
        this.pagePesquisa
      )
        .then((result) => {
          this.ListaConsulta = result.data.result.result;
          this.ListaConsulta.titulo = this.retornaTipoConsulta();
        })
        .catch((error) => {
          console.log(error);
          this.showAlert("error", "Ocorreu um erro na paginação");
        });
    },

    anteriorPagePesquisa() {
      this.pagePesquisa = this.pagePesquisa - 1;
      if (this.typePesquisa === "ficha_clinica") {
        FichaClinicaService.readPagination(
          this.idPaciente,
          this.dataInicial,
          this.dataFinal,
          this.pagePesquisa
        )
          .then((result) => {
            this.ListaConsulta = result.data.result.result;
            this.ListaConsulta.titulo = this.retornaTipoConsulta();
          })
          .catch((erro) => {
            console.log(erro);
            this.showAlert("error", "Ocorreu um erro na paginação");
          });
      } else if (this.typePesquisa === "laudo") {
        LaudoService.read(
          this.dataInicial,
          this.dataFinal,
          this.idPaciente,
          this.pagePesquisa
        )
          .then((result) => {
            this.ListaConsulta = result.data.result.result;
            this.ListaConsulta.titulo = this.retornaTipoConsulta();
          })
          .catch((error) => {
            console.log(error);
            this.showAlert("error", "Ocorreu um erro na paginação");
          });
      }
    },

    async listConsulta() {
      this.ListaConsulta = {};
      if (this.idPaciente != -1) {
        await Pesquisa(this.typePesquisa, {
          idPaciente: this.idPaciente,
          dataInicial: this.dataInicial,
          dataFinal: this.dataFinal,
        })
          .then((result) => {
            if (Object.keys(result.data.result).length === 0) {
              this.showAlert("info", "Nenhuma Informação Encontrada");
            } else {
              this.totalPagePesquisa = Math.ceil(
                result.data.result.total[0].count / 5
              );
              this.ListaConsulta = result.data.result.result;
              this.ListaConsulta.titulo = this.retornaTipoConsulta();
            }
          })
          .catch((error) => {
            console.log(error);
            this.showAlert("error", "Ocorreu um erro na lista de consulta");
          });
      } else {
        this.showAlert("info", "Selecione um Paciente !");
      }
    },

    retornaData(date) {
      return moment(date).format("DD/MM/YYYY");
    },

    pacienteSelectPesquisa() {
      this.$store.commit("PACIENTE_SELECTED", this.selectPesquisa);
      this.ListaConsulta = {};
    },

    pacienteSelect() {
      this.$store.commit("PACIENTE_SELECTED", this.select);
    },

    visualizar(uuid) {
      FichaClinicaService.read(uuid)
        .then((result) => {
          this.fichaClinica = result.data.ficha[0].json_fichaClinica;
          this.uuidFicha = result.data.ficha[0].uuid;
          this.$store.commit("UUID_FICHACLINICA", result.data.ficha[0].uuid);
          this.editar = true;
          this.showAlert("success", "Ficha Clínica Carregada");
        })
        .catch((err) => {
          console.log(err);
        });
    },

    delet(uuid) {
      if (this.typePesquisa === "prescricao_lente") {
        PrescricaoLenteService.delete(uuid)
          .then((result) => {
            if (result.status === 201) {
              this.showAlert("success", "Registro Deletado com Sucesso");
              this.listConsulta();
            }
          })
          .catch(() => {
            this.showAlert("error", "Erro ao deletar registro");
          });
      } else if (this.typePesquisa === "prescricao_oculos") {
        PrescricaoOculosService.delete(uuid)
          .then((result) => {
            if (result.status === 201) {
              this.showAlert("success", "Registro Deletado com Sucesso");
              this.listConsulta();
            }
          })
          .catch(() => {
            this.showAlert("error", "Erro ao deletar registro");
          });
      } else if (this.typePesquisa === "ficha_clinica") {
        FichaClinicaService.delete(uuid)
          .then((result) => {
            if (result.status === 201) {
              this.showAlert("success", "Registro Deletado com Sucesso");
              this.listConsulta();
            }
          })
          .catch(() => {
            this.showAlert("error", "Erro ao deletar registro");
          });
      } else if (this.typePesquisa === "laudo") {
        this.ListaConsulta.titulo = "Laudo";
      } else if (this.typePesquisa === "atestado") {
        this.ListaConsulta.titulo = "Atestado";
      } else if (this.typePesquisa === "encaminhamento") {
        this.ListaConsulta.titulo = "Encaminhamento";
      }
    },

    Paciente(text, uuid) {
      return {
        text: text,
        value: uuid,
      };
    },

    async list() {
      try {
        const paciente = await PacienteService.readAll();
        paciente.data.map((paciente) => {
          this.ListaPaciente.push(
            this.Paciente(paciente.nomePaciente, paciente.uuid)
          );
        });
      } catch (error) {
        this.showAlert("error", "Ocorreu um problema ao lista pacientes");
      }
    },

    readLaudo() {
      LaudoService.read();
    },

    enviarmsg() {
      window.open(
        `https://api.whatsapp.com/send?phone=55${this.telefonePaciente}&text=""`,
        "_blank"
      );
    },

    async saveLaudo() {
      try {
        this.dadosConsulta.idPaciente = this.idPaciente;
        this.dadosConsulta.data = moment().format("YYYY-MM-DD");
        this.dadosConsulta.titulo = "Laudo";
        const consulta = await ServicoConsulta.save(this.dadosConsulta);
        this.laudo.data = moment().format("YYYY-MM-DD");
        this.laudo.idPaciente = this.idPaciente;
        this.laudo.idConsulta = consulta.data.result.idConsulta[0];
        await LaudoService.save(this.laudo);
        this.showAlert("success", "Laudo Registrado com Sucesso");
      } catch (error) {
        this.showAlert("error", "Erro ao registrar Laudo");
      }
    },
  },
  created() {
    this.loadAgendamentos();
    this.list();
    this.readClinica();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Mitr:wght@300&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Lobster&family=Monda:wght@700&display=swap");
.flex {
  display: flex;
  align-items: flex-end;
}

.noemP {
  font-family: "Mitr", sans-serif;
}

.whatsPaciente {
  cursor: pointer;
}

.avatar {
  display: flex;
}

.consulta {
  margin-top: 10px;
  font-family: "Monda", sans-serif;
}

.check {
  display: flex;
  flex-wrap: wrap;
}

#tabelaPac {
  overflow: auto;
}
.dadosPaciente {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.fichaClinica {
  margin-top: 20px;
}

@media (max-width: 700px) {
  .noemP {
    font-size: 25px !important;
  }

  .avt {
    margin-left: 110px;
  }
  #tabelaPac button {
    height: 40px;
    font-size: 6px;
  }

  .dadosPaciente {
    display: flex;
    flex-direction: column;
  }
  .agendadosHoje {
    overflow-y: auto;
  }
  .agendadosHoje table {
    width: 600px;
  }
}
.divFlex {
  border: 1px solid black;
}
</style>