<template>
  <div id="SearchPage">
      <v-sheet width="800" class="rounded-xl">
      <v-form id="SearchForm" class="text-center pa-10 rounded frosted-glass-effect">
          <h1>{{ title }}</h1>
          <v-text-field filled rounded placeholder="Digite o nome do aluno" v-model="search" append-icon="mdi-magnify" class="ma-5" label="Nome do aluno"></v-text-field>
          <div class="d-flex flex-row justify-space-around ">
            <v-radio-group class="flex-1-0 ma-2 pa-2" v-model="filtroCurso">
              <h3 class="mb-2">Curso</h3>
              <v-radio selected color="primary" label="Mestrado" value="ME"></v-radio>
              <v-radio color="primary" label="Doutorado" value="DO"></v-radio>
              <v-radio color="primary" label="DD" value="DD"></v-radio>
            </v-radio-group>
            <v-radio-group class="ma-2 pa-2" v-model="filtroPrograma">
              <h3 class="mb-2">Programa</h3>
              <v-radio color="red" label="CCMC" value="CCMC"></v-radio>
              <v-radio color="red" label="MAT" value="MAT"></v-radio>
              <v-radio color="red" label="PROFMAT" value="PROFMAT"></v-radio>
              <v-radio color="red" label="PIPGEs" value="PIPGEs"></v-radio>
              <v-radio color="red" label="MECAI" value="MECAI"></v-radio>
            </v-radio-group>
          </div>
          <div class="d-flex flex-row">
            <v-btn :ripple="true" color="primary" outlined size="x-large" v-on:click="grabResults" class="flex-1-0 me-auto">Pesquisar</v-btn>

            <v-btn :ripple="true" color="secondary" outlined size="x-large" v-on:click="clearSearch" class="mt-2">Limpar</v-btn>
         </div>
      </v-form>
    </v-sheet>
    <v-data-table v-bind:loading="isLoading" loading-text="Aguarde..." v-bind:headers="tableHeaders" v-bind:items="fmtTeses" v-bind:items-per-page="10" class="elevation-1 mt-5 frosted-glass-effect"  v-bind:search="search" @click:row="mostraIndividual">
      <!-- Slot utilizado porque quero ordenar com o formato
           Data2 MAS quero mostrar o formato originall
       -->
      <template v-slot:[`item.Data2`]="{item}">
        {{item.Data}}
      </template>
      </v-data-table>
      <v-scale-transition>
      <v-dialog v-model="dialog" height="auto" width="auto">
      <v-card class="align-center pa-10 rounded frosted-glass-effect rounded-xl">
          <v-card-title class="headline">Ficha individual</v-card-title>
          <v-card-subtitle class="mt-2">O(A) aluno(a) <b>{{fichaIndividualAtual.Nome}}</b> realizou um curso de <b>{{fichaIndividualAtual.CursoEx}}</b> no programa <b>{{fichaIndividualAtual.Programa}}</b> na data <b>{{fichaIndividualAtual.Data}}</b></v-card-subtitle>
          <v-card-text>
            <h3 class="mb-3">Dados tabelados</h3>
            <p>Nome: {{fichaIndividualAtual.Nome}}</p>
            <p>Curso: {{fichaIndividualAtual.CursoEx}}</p>
            <p>Programa: {{fichaIndividualAtual.Programa}}</p>
            <p>Data: {{fichaIndividualAtual.Data}}</p>
          </v-card-text>
          <v-btn color="primary" outlined v-on:click="dialog = false">fechar</v-btn>
      </v-card>
      </v-dialog>
    </v-scale-transition>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data () {
    return {
      title: 'Pesquisar Registros',
      dialog: false,
      fichaIndividualAtual: false,
      teses: [],
      tesesAvailable: false,
      filtroCurso: null,
      isLoading: false,
      filtroPrograma: null,
      tableHeaders: [
        { text: 'Nome', value: 'Nome' },
        { text: 'Curso', value: 'CursoEx' },
        { text: 'Programa', value: 'Programa' },
        { text: 'Data', value: 'Data2' },
        { text: 'Orientador', value: 'Orientador' }
      ],
      search: ''
    }
  },

  methods: {
    grabResults: function () {
      this.isLoading = true
      fetch('http://thanos.icmc.usp.br:4567/api/v1/defesas').then((data) => (data.json())).then((response) => {
        this.teses = response.items.filter(n => {
          // Filtro responsavel por filtrar por
          // programa/curso
          let verifica1 = true
          let verifica2 = true
          if (this.filtroPrograma !== null) {
            verifica1 = (n.Programa === this.filtroPrograma)
          }

          if (this.filtroCurso !== null) {
            verifica2 = (n.Curso === this.filtroCurso)
          }

          return (verifica1 && verifica2)
        })
        this.tesesAvailable = true
        this.isLoading = false
      })
    },

    clearSearch: function () {
      this.filtroPrograma = null
      this.filtroCurso = null
      this.search = ''
      this.teses = []
    },

    mostraIndividual: function (row) {
      this.fichaIndividualAtual = row
      this.dialog = true
    }
  },

  // A computed property fmtTeses foi utilizado para
  // inverter a data encontrada, inverter o formato
  // para ano/mes/dia e, assim, possibilitar
  // o ordenamento correto
  computed: {
    fmtTeses () {
      return this.teses.map((tese) => {
        return {
          ...tese,
          Data2: tese.Data.split('/').reverse().join(),
          CursoEx: (tese.Curso === 'ME') ? 'Mestrado' : (tese.Curso === 'DO') ? 'Doutorado' : tese.Curso
        }
      })
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');

/* Como quero aplicar a fonte somente ao formulario, aplico um ID a ele e modifico. */
#SearchForm {
  font-family: 'Quicksand';
}

/* Alinhamento no centro da tela etc.*/
#SearchPage {
  margin-left: auto;
  margin-right: auto;
  margin-top: 5vh;
  margin-bottom: 5vh;
  border-radius: 10px 10px 10px 10px;
  padding:3vh;
  border: none;
}

/* Efeito no formulario */
.frosted-glass-effect {
  background:none;
  position: relative;
  background-color: white;
  z-index: 100;
}

.frosted-glass-effect::after {
  position: absolute;
  background-image: url("@/assets/icmc-wallpaper.jpg");
  background-attachment: fixed;
  background-size: cover;
  filter:blur(10px);
  width: 100%;
  height: 100%;
  top:0;
  left:0;
  content: '';
  opacity: 0.2;
  z-index: -1;
}

</style>
