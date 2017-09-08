<template>
  <!--<div class="tab" id="app">
    <h1>{{ msg }}</h1>

    <ul>
      <li><button class="tablinks" name="paciente" v-on:click="verPacientes">Pacientes</button></li>
      <li><button class="tablinks" name="medicamento" v-on:click="verMedicamentos">Medicamentos</button></li>
    </ul>

    <div name="mdPaciente">
      <maestrodetallePaciente v-show="mdPacientesVisible"></maestrodetallePaciente>
    </div>
    <div name="mdMedicamento">
      <maestrodetalleMedicamento v-show="mdMedicamentosVisible"></maestrodetalleMedicamento>
    </div>
  </div>-->


<section style="background:#efefe9;">
        <div class="container">
            <div class="row">
                <div class="board">
                    <!-- <h2>Welcome to IGHALO!<sup>™</sup></h2>-->
                    <div class="board-inner">
                    <ul class="nav nav-tabs" id="myTab">
                    <div class="liner"></div>
                     <li class="active">
                     <a href="#home" data-toggle="tab" title="welcome">
                      <span class="round-tabs one">
                              <i class="glyphicon glyphicon-home"></i>
                      </span> 
                  </a></li>

                  <li><a href="#profile" data-toggle="tab" title="profile">
                     <span class="round-tabs two">
                         <i class="glyphicon glyphicon-user"></i>
                     </span> 
           </a>
                 </li>
                 <li><a href="#messages" data-toggle="tab" title="bootsnipp goodies">
                     <span class="round-tabs three">
                          <i class="glyphicon glyphicon-gift"></i>
                     </span> </a>
                     </li>

                     <li><a href="#settings" data-toggle="tab" title="blah blah">
                         <span class="round-tabs four">
                              <i class="glyphicon glyphicon-comment"></i>
                         </span> 
                     </a></li>

                     <li><a href="#doner" data-toggle="tab" title="completed">
                         <span class="round-tabs five">
                              <i class="glyphicon glyphicon-ok"></i>
                         </span> </a>
                     </li>
                     
                     </ul></div>

                     <div class="tab-content">
                      <div class="tab-pane fade in active" id="home">

                          <h3 class="head text-center">Welcome to Bootsnipp<sup>™</sup> <span style="color:#f48260;">♥</span></h3>
                          <p class="narrow text-center">
                              Lorem ipsum dolor sit amet, his ea mollis fabellas principes. Quo mazim facilis tincidunt ut, utinam saperet facilisi an vim.
                          </p>
                          
                          <p class="text-center">
                    <a href="" class="btn btn-success btn-outline-rounded green"> start using bootsnipp <span style="margin-left:10px;" class="glyphicon glyphicon-send"></span></a>
                </p>
                      </div>
                      <div class="tab-pane fade" id="profile">
                          <h3 class="head text-center">Create a Bootsnipp<sup>™</sup> Profile</h3>
                          <p class="narrow text-center">
                              Lorem ipsum dolor sit amet, his ea mollis fabellas principes. Quo mazim facilis tincidunt ut, utinam saperet facilisi an vim.
                          </p>
                          
                          <p class="text-center">
                    <a href="" class="btn btn-success btn-outline-rounded green"> create your profile <span style="margin-left:10px;" class="glyphicon glyphicon-send"></span></a>
                </p>
                          
                      </div>
                      <div class="tab-pane fade" id="messages">
                          <h3 class="head text-center">Bootsnipp goodies</h3>
                          <p class="narrow text-center">
                              Lorem ipsum dolor sit amet, his ea mollis fabellas principes. Quo mazim facilis tincidunt ut, utinam saperet facilisi an vim.
                          </p>
                          
                          <p class="text-center">
                    <a href="" class="btn btn-success btn-outline-rounded green"> start using bootsnipp <span style="margin-left:10px;" class="glyphicon glyphicon-send"></span></a>
                </p>
                      </div>
                      <div class="tab-pane fade" id="settings">
                          <h3 class="head text-center">Drop comments!</h3>
                          <p class="narrow text-center">
                              Lorem ipsum dolor sit amet, his ea mollis fabellas principes. Quo mazim facilis tincidunt ut, utinam saperet facilisi an vim.
                          </p>
                          
                          <p class="text-center">
                    <a href="" class="btn btn-success btn-outline-rounded green"> start using bootsnipp <span style="margin-left:10px;" class="glyphicon glyphicon-send"></span></a>
                </p>
                      </div>
                      <div class="tab-pane fade" id="doner">
  <div class="text-center">
    <i class="img-intro icon-checkmark-circle"></i>
</div>
<h3 class="head text-center">thanks for staying tuned! <span style="color:#f48260;">♥</span> Bootstrap</h3>
<p class="narrow text-center">
  Lorem ipsum dolor sit amet, his ea mollis fabellas principes. Quo mazim facilis tincidunt ut, utinam saperet facilisi an vim.
</p>
</div>
<div class="clearfix"></div>
</div>

</div>
</div>
</div>
</section>

</template>

<script>

/*
import maestrodetallePaciente from './components/MaestroDetallePaciente.vue'
import maestrodetalleMedicamento from './components/maestrodetalleMedicamento.vue'

export default {
  name: 'app',
  data () {
    return {
      mdPeliculasVisible : false,
      mdEntradasVisible: false,
      msg: 'Bienvenido al Maestro - Detalle',
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
    maestrodetalleEntrada, maestrodetallePelicula
  },

  methods: {

      verEntradas: function()
      {
        if(this.mdEntradasVisible == false)
        {
          this.mdEntradasVisible = true;
          this.mdPeliculasVisible = false;
        }
        else if(this.mdEntradasVisible == true)
        {
          this.mdEntradasVisible = false;
          this.mdPeliculasVisible = false;
        }
        
      },

      verPeliculas: function()
      {
        if(this.mdPeliculasVisible == false)
        {
          this.mdPeliculasVisible = true;
          this.mdEntradasVisible = false;
        }
        else if(this.mdPeliculasVisible == true)
        {
          this.mdPeliculasVisible = false;
          this.mdEntradasVisible = false;
        }
      }
        
    }
}*/
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
