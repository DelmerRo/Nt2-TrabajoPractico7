<template lang="html">

  <section class="src-components-listar-persona">
     <div class="jumbotron">
      <h2>Listado de Personas</h2>
      <hr />
      <br />

      <div v-if="personas.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>NOMBRE</th>
            <th>EDAD</th>
            <th>EMAIL</th>
             <th>ELIMINAR</th>
          </tr>

          <tr
            v-for="(persona, index) in personas"
            :key="index"
          >
            <td>{{ persona.nombre }}</td>
            <td>{{ persona.edad }}</td>
            <td>{{ persona.email }}</td>
              <th> <button
                class="btn btn-danger"
                @click="eliminarPersona(persona.id)"
              >
                Eliminar</button
              ></th>
          </tr>
          
        </table>
         <!-- Botón de envío -->
        
      </div>
      <h3 v-else class="alert alert-info">No hay personas Registrados</h3>
    </div>
    
    <span alert alert-info >Total de registros: {{ calcularHistorialPorNombre.totalEnGeneral }}</span
    >
      <hr>
      <br>
     
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-listar-persona',
    props: [],
    mounted () {
this.getPersonas()
    },
    data () {
      return {
      url: 'https://62861febf0e8f0bb7c10b9cd.mockapi.io/Persona',
        personas: [],
      }
    },
    methods: {
async getPersonas() {
        try {
          let { data: personas } = await this.axios(this.url);
          console.log('AXIOS GET personas', personas);
          this.personas = personas;
        } catch (error) {
          console.error('Error en getPersonas()', error.message);
        }
    },
async eliminarPersona(id){
  
      if (!confirm('¿Desea eliminar este paciente?')) return;
      try {
        let { data: paciente } = await this.axios.delete(this.url + '/' + id);
        console.log('AXIOS DELETE paciente', paciente);

            let index = this.pacientes.findIndex((p) => p.id == paciente.id);
        if (index == -1) throw new Error('paciente no encontrado');
        this.pacientes.splice(index, 1);
      } catch (error) {
        console.error('Error en deletePaciente()', error.message);
      }
      this.getPersonas()
    },
    async irAActualizarPaciente(id) {
      console.log(id);
      this.$router.push({
        path: '/editPatient',
        name: 'editPatient',
        params: { id: id },
      });
}

    },
    computed: {
calcularHistorialPorNombre() {
       
        let total = this.personas.length;
        return {

          totalEnGeneral: total,
          
        };
    },
    }
}


</script>

<style scoped lang="css">
  .src-components-listar-persona {

  }
      .jumbotron {
    background-color:cadetblue;
    color: white;
}

hr {
    background-color: #bbb;
}  

pre {
  color: white;
}

.item { text-align: right; }
.finp { width: 60%; }

label{
display: inline-block;
width: 80px;
}

td{
color:black;
background-color:gainsboro;
}
</style>
