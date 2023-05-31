<template>
    <v-footer absolute border class="mt-5" color="#002c66" dark>
       <p>Disciplina: {{nomeDisciplina}}</p>
       <v-spacer></v-spacer>
       <p>Código: {{codigoDisciplina}}</p>
    </v-footer>
</template>

<script>
export default {
  data () {
    return {
      nomeDisciplina: 'Desenvolvimento Web e Mobile',
      codigoDisciplina: 'SSC0961'
    }
  }
}
</script>
