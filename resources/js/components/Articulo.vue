<template>
    <div>
       <h1 class="text-center">Gestionar Articulos</h1> 
       <hr>

       <!-- Button to Open the Modal -->
<button type="button" class="btn btn-primary my-4" @click="modificar=false; abrirModal();">
  Nuevo
</button>

<!-- The Modal -->
<div class="modal" :class="{mostrar:modal}" >
  <div class="modal-dialog">
    <div class="modal-content">

      <!-- Modal Header -->
      <div class="modal-header">
        <h4 class="modal-title" v-text="tituloModal"></h4>
        <button type="button" class="close" data-dismiss="modal" @click="cerrarModal();">&times;</button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        <div class="my-4">
          <label for="nombre"  >Nombre del Articulo</label>
          <input v-model="articulo.nombre" type="text" class="form-control" id="nombre" placeholder="Nombre del Articulo">
        </div>

        <div class="my-4">
          <label for="descripcion"  > Descripcion de Articulo</label>
          <input v-model="articulo.descripcion" type="text" class="form-control" id="descripcion" placeholder="Descripcion del Articulo">
        </div>

        <div class="my-4">
          <label for="cantidad"  >Cantidad</label>
          <input v-model="articulo.stock" type="number" class="form-control" id="cantidad" placeholder="Cantidad del Articulo">
        </div>
        
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="cerrarModal();">Cancelar</button>
        <button type="button" class="btn btn-success" data-dismiss="modal" @click="guardar();">Guardar</button>
      </div>

    </div>
  </div>
</div>

       <table class="table table-striped">
  <thead class="thead-dark">
    <tr>
      <th scope="col">#</th>
      <th scope="col">Nombre</th>
      <th scope="col">Descripcion</th>
      <th scope="col">Stock</th>
      <th scope="col" colspan="2" class="text-center">Accion</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="art in articulos" :key="art.id">
      <th scope="row" v-text="art.id"></th>
      <td>{{art.nombre}}</td>
      <td>{{art.descripcion}}</td>
      <td>{{art.stock}}</td>
      <td><button class="btn btn-warning" @click="modificar=true; abrirModal(art);">Editar</button> </td>
      <td><button @click="eliminar(art.id)" class="btn btn-danger">Eliminar</button> </td>
    </tr>
    
  </tbody>
</table>



    </div>
</template>
<script>
export default {
    
    data(){
        return{
            modal:0,
            articulos:[],
            tituloModal:'',
            modificar:true,
            articulo:{
              nombre:'aa',
              descripcion:'bb',
              stock:100            
            },
            id:0,
        }
    },
    methods: {
            async listar(){
                const res=await axios.get('/articulos');
                this.articulos=res.data;
            },
            async eliminar(id){
                const res=await axios.delete('/articulos/'+id);
                this.listar();
            },
            abrirModal(data={}){
              if(this.modificar)
              {
                this.id=data.id;
                this.tituloModal='Editar Articulo';
                this.articulo.nombre=data.nombre;
                this.articulo.descripcion=data.descripcion;
                this.articulo.stock=data.stock;
              }
              else
              {
                this.id=0;
                this.tituloModal='Crear Articulo';
                this.articulo.nombre='';
                this.articulo.descripcion='';
                this.articulo.stock=1;
              }
              this.modal=1;
            },
            cerrarModal(){
              this.modal=0;
            },
            async guardar(){
              if(this.modificar)
              {
                const res=await axios.put('/articulos/'+this.id,this.articulo)
              }
              else
              {
                const res=await axios.post('/articulos',this.articulo)

              }
              this.cerrarModal();
              this.listar();
            },
        },
    created(){
        this.listar();
    }
}

</script>
<style >
.mostrar{
    display: list-item;
    opacity: 1;
    background:rgba(25, 25, 26, 0.555);
}

</style>