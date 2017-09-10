<template>
    <div id="maestrodetalleMedicamento">

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
                    <h1 v-show="modoNuevo"> Nueva Medicamento : </h1>
                </li>
                <li>
                    <label for="nombreM"> Nombre: </label>
                    <input type="text" name="nombreM" value="Nombre" v-model="medicamento.Nombre" v-bind:disabled="disable">
                    
                    <label for="tipo"> Tipo: </label>
                    <input type="text" name="tipo" value="Tipo" v-model="medicamento.Tipo" v-bind:disabled="disable">
                   
                    <label for="presentacion"> Presentacion: </label>
                    <input type="text" name="presentacion" value="Presentacion" v-model="medicamento.Presentacion" 
                    v-bind:disabled="disable">
                   
                    <label for="fecha"> Fecha Caducidad: </label>
                    <input type="text" name="fecha" value="Fecha" v-model="medicamento.FechaCaducidad" v-bind:disabled="disable">

                    <button v-on:click="editable(modos.editar)" v-show="btnEditElim">Editar</button>
                    <button v-on:click="editable(modos.eliminar)" v-show="btnEditElim">Eliminar</button>
                    <button v-on:click="cerrarDetalles" v-show="btnEditElim">Cerrar</button>
                    
                    <br>
                    <br>
                    <button v-on:click="crearMedicamento" v-show="btnAceptarCancelar">Aceptar</button>
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
    name: 'maestrodetalleMedicamento',
    //props: ['datapadre'],
    data() {
        return {

            mostrarDetallesContenedor: false,
            disable: true,
            medicamentos: [],
            btnAceptarCancelar: false,
            btnEditElim: false,
            btnACtCancelar: false,
            modoNuevo: false,
            modoDetalle: true,
            medicamento: {Id: "", Nombre: "", Tipo: "", Presentacion: "", FechaCaducidad: ""},
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
                this.eliminarMedicamento();

            }
            else if(this.disable = false)
            {
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

              alert( "Creado el medicamento -> " + "Id: " + data.Id + " Nombre: " + data.Nombre + " Tipo: " + data.Tipo + " Presentacion: " + data.Presentacion + "Fecha de Caducidad" + data.FechaCaducidad);

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
              alert( "ERROR, No se ha podido crear el Medicamento" );
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

              alert( "Eliminado el medicamento -> " + "Id: " + data.Id + " Nombre: " + data.Nombre + " Tipo: " + data.Tipo + " Presentacion: " + data.Presentacion + "Fecha de Caducidad" + data.FechaCaducidad);

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
              alert( "ERROR, al eliminar el Medicamento" );
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

                alert( "Se ha actualizado el Medicamento");
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
                alert( "ERROR, al actualizar el Medicamento" );
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
                        alert( "error" );
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
                    alert( "error" );
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
