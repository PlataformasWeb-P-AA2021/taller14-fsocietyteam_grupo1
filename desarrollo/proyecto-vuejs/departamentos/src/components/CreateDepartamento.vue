<template>
    <div class="pt-5">
        <form @submit.prevent="create" method="post">
            <div class="form-group">
                <label for="nomPropietario">Nombre del Propietario</label>
                <input
                    type="text"
                    class="form-control"
                    id="nomPropietario"
                    v-model="departamento.nomPropietario"
                    v-validate="'required'"
                    name="nomPropietario"
                    placeholder="Ingrese el nombre de propietario"
                    :class="{'is-invalid': errors.has('departamento.nomPropietario') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid name.
                </div>
            </div>

            <div class="form-group">
                <label for="costo">Costo</label>
                <input
                    type="text"
                    class="form-control"
                    id="costo"
                    v-model="departamento.costo"
                    v-validate="'required'"
                    name="costo"
                    placeholder="Ingrese el costo del departamento"
                    :class="{'is-invalid': errors.has('departamento.costo') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid cost.
                </div>
            </div>

            <div class="form-group">
                <label for="num_cuartos">Número de cuartos</label>
                <input
                    type="text"
                    class="form-control"
                    id="num_cuartos"
                    v-model="departamento.num_cuartos"
                    v-validate="'required'"
                    name="num_cuartos"
                    placeholder="Ingrese el número de cuartos"
                    :class="{'is-invalid': errors.has('departamento.num_cuartos') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid numer's rooms.
                </div>
            </div>

            <div class="form-group">
              <br>
                <label for="edificio">Edificio</label>
                <select v-model="departamento.edificio">
                            <option v-for="e in edificiosList" :key="e.url" :value="e.url">{{ e.nombre }}</option>
                        </select>
            </div>
            
            <button type="submit" class="btn btn-primary">Crear</button>
        </form>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data() {
        return {
            departamento: {
                nomPropietario: '',
                costo: '',
                num_cuartos: '',
                edificio: '',
            },
            edificiosList: [],
            submitted: false
        }
    },
    mounted() {
        this.getEdificiosList()
    },
    methods: {
      getEdificiosList() {
            axios
            .get('http://127.0.0.1:8000/api/edificios/')
            .then(response => {
                this.edificiosList = response.data.results
            })
            .catch(error => {
                console.log(error)
            })

        },
        create: function (e) {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                }
                axios.post('http://127.0.0.1:8000/api/departamentos/',
                        this.departamento
                    )
                    .then(response => {
                        this.$router.push('/');
                    })
            });
        }
    },
}
</script>