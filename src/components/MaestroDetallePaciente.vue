<template>
    <div id="maestrodetallePaciente">

        <div class="alert alert-success alert-dismissable" v-show="this.msgOk" style="font-size: 18px; bold;">
          <button type="button" class="close btn-sm" data-dismiss="alert" v-on:click.stop.prevent = "nuevaFuncion">&times;</button>
          <i class="glyphicon glyphicon-ok"> &nbsp; </i> <strong>{{msg}}</strong>
        </div>

        <div class="alert alert-danger alert-dismissable" v-show="this.msgKo" style="font-size: 16px;">
          <button type="button" class="close" data-dismiss="alert" v-on:click.stop.prevent = "nuevaFuncion">&times;</button>
          <i class="glyphicon glyphicon-remove"> &nbsp; </i> <strong>{{msg}}</strong>
        </div>


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

                    <br>

                    <label for="edad"> Edad: </label>
                    <input type="numeric" name="edad" value="Edad" v-model="paciente.Edad" v-bind:disabled="disable">
                   
                    <label for="sexo"> Sexo: </label>
                    <input type="radio" name="sexo" v-bind:disabled="disable" v-model="paciente.Sexo" value="Hombre"  :checked="paciente.Sexo"> Hombre
                    <input type="radio" name="sexo" v-bind:disabled="disable" v-model="paciente.Sexo" value="Mujer" :checked="!paciente.Sexo"> Mujer
                   
                    <br>

                    <label for="descripcion"> Descripcion de la Dolencia: </label>
                    <br>
                    <textarea rows="3" style="background: white; resize: none; overflow: auto; text-overflow: ellipsis" 
                    type="string" name="descripcion" v-bind:disabled="disable" v-model="paciente.DescripcionDolencia"> </textarea>

                    <br>

                    <label for="duracion"> Duracion del tratamiento: (Dias) </label>
                    <input type="numeric" name="duracion" value="Duracion" v-model="paciente.DuracionTratamiento" 
                    v-bind:disabled="disable">

                    <br>

                    <button v-on:click="editable(modos.editar)" v-show="btnEditElim">Editar</button>
                    <button v-on:click="editable(modos.eliminar)" v-show="btnEditElim">Eliminar</button>
                    <button v-on:click="cerrarDetalles" v-show="btnEditElim">Cerrar</button>
                    
                    <br>
                    <br>
                    <button v-on:click="validarPaciente(modos.crear)" v-show="btnAceptarCancelar">Aceptar</button>
                    <button v-on:click="cancelar" v-show="btnAceptarCancelar">Cancelar</button>

                    <br>
                    <button v-on:click="validarPaciente(modos.actualizar)" v-show="btnACtCancelar">Actualizar</button>
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

            msg: "",
            msgOk: false,
            msgKo: false,
            mostrarDetallesContenedor: false,
            disable: true,
            pacientes: [],
            btnAceptarCancelar: false,
            btnEditElim: false,
            btnACtCancelar: false,
            modoNuevo: false,
            modoDetalle: true,
            paciente: {Id: "", Nombre: "", Apellidos: "", Sexo: "", Edad: "", DescripcionDolencia: "", DuracionTratamiento: ""},
            modos: {editar: "editar", eliminar: "eliminar", nuevo: "nuevo", crear : "crear", actualizar : "actualizar"}
        };
    },
    methods: {

        editable: function(mode)
        {
            if(this.disable == true && mode == "editar")
            {
                this.msgKo = false;
                this.msgOk = false;
                this.disable = false;
                this.btnAceptarCancelar = false;
                this.btnEditElim = false;
                this.btnACtCancelar = true;
               // this.actualizar();
            }
            else if(this.disable == true && mode == "nuevo")
            {
                this.msgKo = false;
                this.msgOk = false;
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
                this.msgKo = false;
                this.msgOk = false;
                this.eliminarPaciente();

            }
            else if(this.disable = false)
            {
                this.msgKo = false;
                this.msgOk = false;
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

              _this.refreshList();
              _this.mostrarMsgOk("creado");
              _this.msgKo = false;
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.btnACtCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;
              

          })
          .fail(function(data) {
              _this.mostrarMsgKo("creado");
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

              _this.refreshList();
              _this.mostrarMsgOk("eliminado");
              _this.msgKo = false;
              _this.limpiarCampos();
              _this.modoNuevo = false;
              _this.modoDetalle = true;
              _this.btnAceptarCancelar = false;
              _this.btnACtCancelar = false;
              _this.disable = true;
              _this.mostrarDetallesContenedor = false;

            })
            .fail(function(data) {
              _this.mostrarMsgKo("eliminado");
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

                _this.refreshList();
                _this.mostrarMsgOk("actualizado");
                _this.msgKo = false;
                _this.limpiarCampos();
                _this.modoNuevo = false;
                _this.modoDetalle = true;
                _this.btnAceptarCancelar = false;
                _this.btnACtCancelar = false;
                _this.disable = true;
                _this.mostrarDetallesContenedor = false;
                
            })
            .fail(function(data) {
                _this.mostrarMsgKo("actualizado");
            });

        },

        mostrarDetalles: function(id)
        {

                this.msgKo = false;
                this.msgOk = false;

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
                        _this.mostrarMsgKo("error");
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
                    _this.mostrarMsgKo( "error" );
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
            _this.msgKo = false;
            _this.msgOk = false;
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
            _this.msgKo = false;
            _this.msgOk = false;
            _this.modoNuevo = false;
            _this.modoDetalle = true;
            _this.btnAceptarCancelar = false;
            _this.btnACtCancelar = false;
            _this.disable = true;
            _this.mostrarDetallesContenedor = false;
        },

        mostrarMsgOk: function(data)
        {

          if(data == "creado")
          {
            this.msg = "¡ El Paciente ha sido creado con éxito !"
            this.msgOk = true;

          }else if (data == "actualizado")
          {
            
            this.msg = "¡ El Paciente ha sido actualizado con éxito !"
            this.msgOk = true;

          } else if(data == "eliminado")
          {
           
            this.msg = "¡ El Paciente se ha eliminado con éxito !"
            this.msgOk = true;
            
          } else if(data == "error")
          {
            
            this.msg = "¡ Ha ocurrido un error, vuelva a intentarlo !"
            this.msgOk = true;
            
          }

        },

        mostrarMsgKo: function(data)
        {
          if(data == "creado")
          {
            this.msg = "Error al crear el Paciente, vuelva a intentarlo."
            this.msgKo = true;

          }else if (data == "actualizado")
          {
            
            this.msg = "Error al actualizar el Paciente, vuelva a intentarlo."
            this.msgKo = true;

          } else if(data == "eliminado")
          {
           
            this.msg = "Error al eliminar el Paciente, vuelva a intentarlo."
            this.msgKo = true;
            
          } else if(data == "error")
          {
            
            this.msg = "Se ha producido un error, vuelva a intentarlo."
            this.msgKo = true;
            
          }

        },

        validarPaciente: function(data)
        {
          if(this.paciente.Nombre == '')
          {
            this.msg = "El nombre del paciente no puede estar vacío."
            this.msgKo = true;
          }
          else if (this.paciente.Nombre.length <= 1 || this.paciente.Nombre.length >= 31)
          {
            this.msg = "El nombre del paciente debe tener entre 1 y 30 caracteres."
            this.msgKo = true;
          }
          else if (this.paciente.Apellidos == '')
          {
            this.msg = "Los apellidos del paciente no pueden estar vacíos."
            this.msgKo = true;
          }
          else if (this.paciente.Sexo == '')
          {
            this.msg = "Debe seleccionar el sexo del paciente."
            this.msgKo = true;
          }
          else if ( !this.isInt(this.paciente.Edad) || this.paciente.Edad == '' || this.paciente.Edad < 1 || this.paciente.Edad > 99)
          {
            this.msg = "La edad del paciente debe ser un numero entre 1 y 100."
            this.msgKo = true;
          }
          else if (this.paciente.DescripcionDolencia == '')
          {
            this.msg = "Por favor, escriba brevemente la dolencia del paciente."
            this.msgKo = true;
          }
          else if (!this.isInt(this.paciente.DuracionTratamiento) || this.paciente.DuracionTratamiento == '')
          {
            this.msg = "La duracion del tratamiento no es correcta, introduzca la duracion en días."
            this.msgKo = true;
          }
          else
          {
            if(data == "crear")
            {
              this.crearPaciente();
            }
            else if(data == "actualizar")
            {
              this.actualizar();
            }
            
          }
        },

        nuevaFuncion: function()
        {
          this.msgOk = false;
          this.msgKo = false;
        },

        isInt: function(n) {
          return n % 1 === 0;
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
                    _this.mostrarMsgKo( "error" );
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