<template>
    <v-sheet id="GroupPage" width="800" height="400" class="rounded-xl frosted-glass-effect pa-5">
      <h1 class="text-center">{{title}}</h1>
      <ul class="ma-10">
        <li v-bind:key="integrante.NUSP" v-for="integrante in integrantes">{{integrante.Nome}} : {{integrante.NUSP}}</li>
      </ul>
    </v-sheet>
</template>

<script>
export default {
  name: 'HomePage',
  data () {
    return {
      title: 'Trabalho 1 - Desenvolvimento Web',
      integrantes: [
        {
          Nome: 'Leonardo Gonçalves Chahud',
          NUSP: '5266649'
        }
      ]
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');

/* Como quero aplicar a fonte somente ao formulario, aplico um ID a ele e modifico. */
#GroupPage {
  font-family: 'Quicksand';
}

/* Alinhamento no centro da tela etc.*/
#GroupPage {
  margin-left: auto;
  margin-right: auto;
  margin-top: 10vh;
  margin-bottom: 5vh;
  border-radius: 10px 10px 10px 10px;
  padding:3vh;
  border: 1px solid rgba(0,0,0,0.2);
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
