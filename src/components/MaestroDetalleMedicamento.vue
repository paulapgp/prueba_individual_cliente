<template>
    <div id="maestrodetalleMedicamento">

       <div class="alert alert-success alert-dismissable" v-show="this.msgOk" style="font-size: 18px; bold;">
          <button type="button" class="close btn-sm" data-dismiss="alert" v-on:click.stop.prevent = "nuevaFuncion">&times;</button>
          <i class="glyphicon glyphicon-ok"> &nbsp; </i> <strong>{{msg}}</strong>
        </div>

        <div class="alert alert-danger alert-dismissable" v-show="this.msgKo" style="font-size: 16px;">
          <button type="button" class="close" data-dismiss="alert" v-on:click.stop.prevent = "nuevaFuncion">&times;</button>
          <i class="glyphicon glyphicon-remove"> &nbsp; </i> <strong>{{msg}}</strong>
        </div>


        <div name="listaMedicamentos">
            <ul>
                <li>
                    <h1>Medicamentos</h1>
                </li>
                <li v-for="medicamento in medicamentos">
                    <label for="nombre" v-on:click="mostrarDetalles(medicamento.Id)"> Medicamento: </label>
                    <input type="text" name="nombre" v-model="medicamento.Nombre" disabled="true"> 
                </li> 
            </ul>

            <br>
            <button v-on:click="editable(modos.nuevo)">Nuevo Medicamento</button>
            <br>

        </div>

        <div name="detalles" v-show="mostrarDetallesContenedor">
            <ul>
                <li>
                    <h1 v-show="modoDetalle"> Medicamento : </h1>
                </li>
                <li>
                    <h1 v-show="modoNuevo"> Nuevo Medicamento : </h1>
                </li>
                <li>
                    <label for="nombreM"> Nombre: </label>
                    <input type="text" name="nombreM" value="Nombre" v-model="medicamento.Nombre" v-bind:disabled="disable">
                    
                    <label for="tipo"> Tipo: </label>
                    <input type="text" name="tipo" value="Tipo" v-model="medicamento.Tipo" v-bind:disabled="disable">
                   
                    <label for="presentacion"> Presentacion: </label>
                    <select class="w3-select w3-border" name="presentacion" value="Presentacion" v-model="medicamento.Presentacion" 
                    v-bind:disabled="disable">
                      <option value="capsulas">Capsulas</option>
                      <option value="comprimidos">Comprimidos</option>
                      <option value="sobres">Sobres</option>
                      <option value="pomadas">Pomadas</option>
                      <option value="inyecciones">Inyecciones</option>
                      <option value="supositorios">Supositorios</option>
                    </select>


                    <label for="fecha"> Fecha Caducidad: </label>
                    <input class="w3-input w3-border" type="date" name="fecha" value="Fecha" v-bind:disabled="disable" 
                    v-model="medicamento.FechaCaducidad">


                    <button v-on:click="editable(modos.editar)" v-show="btnEditElim">Editar</button>
                    <button v-on:click="editable(modos.eliminar)" v-show="btnEditElim">Eliminar</button>
                    <button v-on:click="cerrarDetalles" v-show="btnEditElim">Cerrar</button>
                    
                    <br>
                    <br>
                    <button v-on:click="validarMedicamento(modos.crear)" v-show="btnAceptarCancelar">Aceptar</button>
                    <button v-on:click="cancelar" v-show="btnAceptarCancelar">Cancelar</button>

                    <br>
                    <button v-on:click="validarMedicamento(modos.actualizar)" v-show="btnACtCancelar">Actualizar</button>
                    <button v-on:click="cancelar" v-show="btnACtCancelar">Cancelar</button>

                </li> 
            </ul>
        </div>

    </div>
</template>

<script>

export default {
    name: 'maestrodetalleMedicamento',
    //props: ['datapadre'],
    data() {
        return {

            msg: "",
            msgOk: false,
            msgKo: false,
            mostrarDetallesContenedor: false,
            disable: true,
            medicamentos: [],
            btnAceptarCancelar: false,
            btnEditElim: false,
            btnACtCancelar: false,
            modoNuevo: false,
            modoDetalle: true,
            medicamento: {Id: "", Nombre: "", Tipo: "", Presentacion: "", FechaCaducidad: ""},
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
                this.eliminarMedicamento();

            }
            else if(this.disable = false)
            {
                this.msgKo = false;
                this.msgOk = false;
                this.disable = true;
            }
        },

        crearMedicamento: function()
        {
            var _this = this;

                $.ajax({
                  type: "POST",
                  url: "http://localhost:51847/api/Medicamentos",
                  data: { Nombre: _this.medicamento.Nombre, Tipo: _this.medicamento.Tipo, Presentacion: _this.medicamento.Presentacion, FechaCaducidad: _this.medicamento.FechaCaducidad},
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

        eliminarMedicamento: function()
        {
             var _this = this;

            $.ajax({

              type: "DELETE",
              url: "http://localhost:51847/api/Medicamentos/" + _this.medicamento.Id,
              data: { Id: _this.medicamento.Id }

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
            url: "http://localhost:51847/api/Medicamentos/" + _this.medicamento.Id,
            data: _this.medicamento,

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
                  url : "http://localhost:51847/api/Medicamentos/" + id,
                  type: "GET",
                })
                .done(function(data) {

                  _this.medicamento.Id = data.Id;
                  _this.medicamento.Nombre = data.Nombre;
                  _this.medicamento.Tipo = data.Tipo;
                  _this.medicamento.Presentacion = data.Presentacion;
                  _this.medicamento.FechaCaducidad = data.FechaCaducidad;

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
              url : "http://localhost:51847/api/Medicamentos",
              type: "GET",
            })
            .done(function(data) {

              _this.medicamentos = data;
            })
            .fail(function(data) {
                    _this.mostrarMsgKo( "error" );
                  });
          },

        limpiarCampos: function()
        {
            this.medicamento = {Id: "", Nombre: "", Tipo: "", Presentacion: "", FechaCaducidad: ""};
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
          //var _this = this;

          if(data == "creado")
          {
            this.msg = "¡ El Medicamento ha sido creado con éxito !"
            this.msgOk = true;

          }else if (data == "actualizado")
          {
            
            this.msg = "¡ El Medicamento ha sido actualizado con éxito !"
            this.msgOk = true;

          } else if(data == "eliminado")
          {
           
            this.msg = "¡ El Medicamento se ha eliminado con éxito !"
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
            this.msg = "Error al crear el Medicamento, vuelva a intentarlo."
            this.msgKo = true;

          }else if (data == "actualizado")
          {
            
            this.msg = "Error al actualizar el Medicamento, vuelva a intentarlo."
            this.msgKo = true;

          } else if(data == "eliminado")
          {
           
            this.msg = "Error al eliminar el Medicamento, vuelva a intentarlo."
            this.msgKo = true;
            
          } else if(data == "error")
          {
            
            this.msg = "Se ha producido un error, vuelva a intentarlo."
            this.msgKo = true;
            
          }

        },

        validarMedicamento: function(data)
        {
          if(this.medicamento.Nombre == '')
          {
            this.msg = "El nombre del medicamento no puede estar vacío."
            this.msgKo = true;
          }
          else if (this.medicamento.Nombre.length <= 0 || this.medicamento.Nombre.length >= 31)
          {
            this.msg = "El nombre del medicamento debe tener entre 1 y 30 caracteres."
            this.msgKo = true;
          }
          else if (this.medicamento.Tipo == '')
          {
            this.msg = "El tipo del medicamento no puede estar vacío."
            this.msgKo = true;
          }
          else if (this.medicamento.Presentacion == '')
          {
            this.msg = "Debe seleccionar un valor de presentacion del medicamento."
            this.msgKo = true;
          }
          else if (this.medicamento.FechaCaducidad == '')
          {
            this.msg = "La fecha de caducidad no es correcta, seleccione una de nuevo."
            this.msgKo = true;
          }
          else
          {
            if(data == "crear")
            {
              this.crearMedicamento();
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
        }

    },

   mounted: function() {
          var _this = this;
          $.ajax(
            {
              url : "http://localhost:51847/api/Medicamentos",
              type: "GET",
            })
            .done(function(data) {
              _this.medicamentos = data;
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
