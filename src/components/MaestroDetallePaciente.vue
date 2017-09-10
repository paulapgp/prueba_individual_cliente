<template>
    <div id="maestrodetallePaciente">

        <div name="listaPacientes">
            <ul>
                <li>
                    <h1>Pacientes</h1>
                </li>
                <li v-for="paciente in pacientes">
                    <label for="nombre" v-on:click="mostrarDetalles(paciente.Id)"> Nombre: </label>
                    <input type="text" name="nombre" v-model="paciente.Nombre" disabled="true"> 
                    <label for="apellidos"> Apellidos: </label>
                    <input type="text" name="apellidos" v-model="paciente.Apellidos" disabled="true">
                </li> 
            </ul>

            <br>
            <button v-on:click="editable(modos.nuevo)">Nuevo Paciente</button>
            <br>

        </div>

        <div name="detalles" v-show="mostrarDetallesContenedor">
            <ul>
                <li>
                    <h1 v-show="modoDetalle"> Paciente : </h1>
                </li>
                <li>
                    <h1 v-show="modoNuevo"> Nuevo Paciente : </h1>
                </li>
                <li>
                    <label for="nombreP"> Nombre: </label>
                    <input type="text" name="nombreP" value="Nombre" v-model="paciente.Nombre" v-bind:disabled="disable">
                    
                    <label for="apellidos"> Apellidos: </label>
                    <input type="text" name="apellidos" value="Apellidos" v-model="paciente.Apellidos" v-bind:disabled="disable">
                   
                    <label for="sexo"> Sexo: </label>
                    <input type="text" name="sexo" value="Sexo" v-model="paciente.Sexo" v-bind:disabled="disable">
                   
                    <label for="edad"> Edad: </label>
                    <input type="numeric" name="edad" value="Edad" v-model="paciente.Edad" v-bind:disabled="disable">

                    <label for="descripcion"> Descripcion de la Dolencia: </label>
                    <input type="text" name="descripcion" value="Descripcion" v-model="paciente.DescripcionDolencia" 
                    v-bind:disabled="disable">

                    <label for="duracion"> Duracion del tratamiento: (Dias) </label>
                    <input type="numeric" name="duracion" value="Duracion" v-model="paciente.DuracionTratamiento" 
                    v-bind:disabled="disable">

                    <button v-on:click="editable(modos.editar)" v-show="btnEditElim">Editar</button>
                    <button v-on:click="editable(modos.eliminar)" v-show="btnEditElim">Eliminar</button>
                    <button v-on:click="cerrarDetalles" v-show="btnEditElim">Cerrar</button>
                    
                    <br>
                    <br>
                    <button v-on:click="crearPaciente" v-show="btnAceptarCancelar">Aceptar</button>
                    <button v-on:click="cancelar" v-show="btnAceptarCancelar">Cancelar</button>

                    <br>
                    <button v-on:click="actualizar" v-show="btnACtCancelar">Actualizar</button>
                    <button v-on:click="cancelar" v-show="btnACtCancelar">Cancelar</button>

                </li> 
            </ul>
        </div>

    </div>
</template>

<script>

export default {
    name: 'maestrodetallePaciente',
    //props: ['datapadre'],
    data() {
        return {

            mostrarDetallesContenedor: false,
            disable: true,
            pacientes: [],
            btnAceptarCancelar: false,
            btnEditElim: false,
            btnACtCancelar: false,
            modoNuevo: false,
            modoDetalle: true,
            paciente: {Id: "", Nombre: "", Apellidos: "", Sexo: "", Edad: "", DescripcionDolencia: "", DuracionTratamiento: ""},
            modos: {editar: "editar", eliminar: "eliminar", nuevo: "nuevo"}
        };
    },
    methods: {

        editable: function(mode)
        {
            if(this.disable == true && mode == "editar")
            {
                this.disable = false;
                this.btnAceptarCancelar = false;
                this.btnEditElim = false;
                this.btnACtCancelar = true;
               // this.actualizar();
            }
            else if(this.disable == true && mode == "nuevo")
            {
                this.modoNuevo = true;
                this.modoDetalle = false;
                this.mostrarDetallesContenedor = true;
                this.disable = false;
                this.btnEditElim = false;
                this.btnAceptarCancelar = true;
                this.btnACtCancelar = false;
                this.limpiarCampos();
            }
            else if(this.disable == true && mode == "eliminar")
            {
                //this.disable = false;
                this.eliminarPaciente();

            }
            else if(this.disable = false)
            {
                this.disable = true;
            }
        },

        crearPaciente: function()
        {
            var _this = this;

            $.ajax({
              type: "POST",
              url: "http://localhost:51847/api/Pacientes",
              data: { Nombre: _this.paciente.Nombre, Apellidos: _this.paciente.Apellidos, Sexo: _this.paciente.Sexo, 
                Edad: _this.paciente.Edad, DescripcionDolencia: _this.paciente.DescripcionDolencia, 
                DuracionTratamiento: _this.paciente.DuracionTratamiento},
            })
            .done(function(data) {

              alert( "Creado el paciente -> " + "Id: " + data.Id + " Nombre: " + data.Nombre + " Apellidos: " + data.Apellidos + " Sexo: " + data.Sexo + " Edad" + data.edad);

              _this.refreshList();
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.btnACtCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;
              

          })
          .fail(function(data) {
              alert( "ERROR, No se ha podido crear el Paciente" );
          });

        },

        eliminarPaciente: function()
        {
             var _this = this;

            $.ajax({

              type: "DELETE",
              url: "http://localhost:51847/api/Pacientes/" + _this.paciente.Id,
              data: { Id: _this.paciente.Id }

            })
            .done(function(data) {

              alert( "Eliminado el paciente -> " + "Id: " + data.Id + " Nombre: " + data.Nombre + " Apellidos: " + data.Apellidos + " Sexo: " + data.Sexo + " Edad" + data.edad);

              _this.refreshList();
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.btnACtCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;

            })
            .fail(function(data) {
              alert( "ERROR, al eliminar el Paciente" );
          });

        },

        actualizar: function()
        {
            var _this = this;

            $.ajax({

            type: "PUT",
            url: "http://localhost:51847/api/Pacientes/" + _this.paciente.Id,
            data: _this.paciente,

            })
            .done(function(data) {

                alert( "Se ha actualizado el Paciente");
                _this.refreshList();
                _this.limpiarCampos();
                _this.modoNuevo = false;
                _this.modoDetalle = true;
                _this.btnAceptarCancelar = false;
                _this.btnACtCancelar = false;
                _this.disable = true;
                _this.mostrarDetallesContenedor = false;
                
            })
            .fail(function(data) {
                alert( "ERROR, al actualizar el Paciente" );
            });

        },

        mostrarDetalles: function(id)
        {

                if (this.mostrarDetallesContenedor == false) 
                {
                    this.mostrarDetallesContenedor = true;
                    this.btnEditElim = true;
                }

                var _this = this;
                $.ajax(
                {
                  url : "http://localhost:51847/api/Pacientes/" + id,
                  type: "GET",
                })
                .done(function(data) {

                  _this.paciente.Id = data.Id;
                  _this.paciente.Nombre = data.Nombre;
                  _this.paciente.Apellidos = data.Apellidos;
                  _this.paciente.Sexo = data.Sexo;
                  _this.paciente.Edad = data.Edad;
                  _this.paciente.DescripcionDolencia = data.DescripcionDolencia;
                  _this.paciente.DuracionTratamiento = data.DuracionTratamiento;

                })
                .fail(function(data) {
                        alert( "error" );
                      });
          },

          refreshList: function()
          {

           var _this = this;
          $.ajax(
            {
              url : "http://localhost:51847/api/Pacientes",
              type: "GET",
            })
            .done(function(data) {
              _this.pacientes = data;
            })
            .fail(function(data) {
                    alert( "error" );
                  });
          },

        limpiarCampos: function()
        {
            this.paciente = {Id: "", Nombre: "", Apellidos: "", Sexo: "", Edad: "", DescripcionDolencia: "", DuracionTratamiento: ""};
        },

        cancelar: function()
        {
            var _this = this;

            _this.refreshList();
            _this.limpiarCampos();
            _this.modoNuevo = false;
            _this.modoDetalle = true;
            _this.btnAceptarCancelar = false;
            _this.btnACtCancelar = false;
            _this.disable = true;
            _this.mostrarDetallesContenedor = false;
        },

        cerrarDetalles: function()
        {
            var _this = this;

            _this.refreshList();
            _this.limpiarCampos();
            _this.modoNuevo = false;
            _this.modoDetalle = true;
            _this.btnAceptarCancelar = false;
            _this.btnACtCancelar = false;
            _this.disable = true;
            _this.mostrarDetallesContenedor = false;
        }

    },

   mounted: function() {
          var _this = this;
          $.ajax(
            {
              url : "http://localhost:51847/api/Pacientes",
              type: "GET",
            })
            .done(function(data) {
              _this.pacientes = data;
            })
            .fail(function(data) {
                    alert( "error" );
                  });

        }
}

</script>

<style scoped>
#maestro {
    width: 50%;
}

p {

    text-decoration: overline;
}

h1{
    width:100%;
}

ul {
    width: 100%;
}

li {
    list-style-type: none;
    width:100%;
}

li:hover {
    cursor: pointer;
}

.selected{
    background-color:brown;
}
</style>