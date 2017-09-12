<template>
  <div class="tab" id="app">
    <div class="p-3 mb-2 bg-primary text-white">
    <div class="row">

    <h1>Recetas Electrónicas</h1>

    <div class="board-inner">

    <ul class="nav nav-tabs" id="myTab">

       <div class="btn-group btn-group-lg">

      <li>
        <button class="tablinks" name="home">
          <span class="round-tabs one">
            <i class="glyphicon glyphicon-home"></i>
          </span>
        </button>
      </li>

       <li>
        <button class="tablinks" name="medicamentos" v-on:click="verMedicamentos">
          <span class="round-tabs one">
            <i class="glyphicon glyphicon-barcode"></i>
          </span>
        </button>
      </li>

       <li>
        <button class="tablinks" name="pacientes" v-on:click="verPacientes">
          <span class="round-tabs one">
            <i class="glyphicon glyphicon-user"></i>
          </span>
        </button>
      </li>

      </div>

    </ul>

    </div>
    </div>
    </div>

      <maestrodetallePaciente v-show="mdPacientesVisible"></maestrodetallePaciente>
      <maestrodetalleMedicamento v-show="mdMedicamentosVisible"></maestrodetalleMedicamento>

    
  </div>

</template>

<script>


import maestrodetallePaciente from './components/MaestroDetallePaciente.vue'
import maestrodetalleMedicamento from './components/MaestroDetalleMedicamento.vue'

export default {
  name: 'app',
  data () {
    return {
      mdPacientesVisible : false,
      mdMedicamentosVisible: false,
      datos: {
        //disable: true;
        //entradas: [],
        //peliculas: [],
        //entrada: { Precio: "", Sala: "", Fila: "", Butaca: "", FechaEmision: "", Pelicula: "" },
        //pelicula: { Nombre: "", Director: "", Categoria: "", FechaEstreno: "", Pais: "", Duracion: ""}

      }
    }
  },
  components: {
    maestrodetalleMedicamento, maestrodetallePaciente
  },

  methods: {

      verPacientes: function()
      {
        if(this.mdPacientesVisible == false)
        {
          this.mdPacientesVisible = true;
          this.mdMedicamentosVisible = false;
        }
        else if(this.mdPacientesVisible == true)
        {
          this.mdPacientesVisible = false;
          this.mdMedicamentosVisible = false;
        }
        
      },

      verMedicamentos: function()
      {
        if(this.mdMedicamentosVisible == false)
        {
          this.mdMedicamentosVisible = true;
          this.mdPacientesVisible = false;
        }
        else if(this.mdMedicamentosVisible == true)
        {
          this.mdMedicamentosVisible = false;
          this.mdPacientesVisible = false;
        }
      }
        
    }
}
</script>


<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}


.tabcontent {
    display: none;
    padding: 6px 12px;
    border: 1px solid #ccc;
    border-top: none;
}

div.tab button.active {
    background-color: #ccc;
}

div.tab button:hover {
    background-color: #ddd;
}

div.tab {
    overflow: hidden;
    border: 1px solid #ccc;
    background-color: #f1f1f1;
}

div.tab button {
    background-color: inherit;
    font-size: 20px;
    float: left;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 14px 16px;
    transition: 0.3s;
}


</style>
