<template>
    <h1>Ver Anotación</h1>
    
    <p v-if="anotacion == null">Aún no carga</p>
    <div v-if="anotacion != null">
        <hr />
        <dl class="row">
            <dt class="col-sm-2">
                Id
            </dt>
            <dd class="col-sm-10">
                {{anotacion.id}}
            </dd>
            <dt class="col-sm-2">
                Tipo
            </dt>
            <dd class="col-sm-10">
                {{anotacion.tipo}}
            </dd>
            <dt class="col-sm-2">
                Fecha Emisión
            </dt>
            <dd class="col-sm-10">
                {{anotacion.fechaEmision}}
            </dd>
            <dt class="col-sm-2">
                Descripción
            </dt>
            <dd class="col-sm-10">
                {{anotacion.descripcion}}
            </dd>
            <dt class="col-sm-2">
                Estudiante
            </dt>
            <dd v-if="estudiante != null" class="col-sm-10">
                {{obtenerNombreApellidoEstudiante(anotacion.estudianteId)}}
            </dd>
        </dl>
    </div>

    <div v-if="anotacion != null" class="btn-group">
        <a v-bind:href="'/Anotaciones/Editar/'+ anotacion.id " class="btn btn-primary active" aria-current="page">Editar</a>
        <a href="/Anotaciones/" class="btn btn-primary">Volver a la Lista</a>
    </div>

</template>


<script>
    import axios from 'axios'

    const url_estudiantes = "https://localhost:7073/api/EstudiantesAPI"
    const url_anotaciones = "https://localhost:7073/api/AnotacionsAPI"

    export default {
        name: "indice",
        data() {
            return {
                anotacion: null,
                estudiante: null
            }
        },
        methods: {
            obtenerAnotacion() {
                axios.get(url_anotaciones + "/" + this.$route.params.id).then(response => {
                    console.log(response);
                    this.anotacion = response.data;
                    this.estudiante = this.obtenerEstudiante(this.anotacion.estudianteId);
                }).catch(error => console.log(error))
            },
            obtenerNombreApellidoEstudiante(id) {
                var strNombreApellido = this.estudiante.nombre + " " + this.estudiante.apellido;
                return strNombreApellido;
            },
            obtenerEstudiante(index) {
                axios.get(url_estudiantes + "/" + index).then(response => {
                    console.log(response);
                    this.estudiante = response.data;
                }).catch(error => console.log(error))
            }
        },
        created: function () {
            this.obtenerAnotacion();
        }
    }
</script>