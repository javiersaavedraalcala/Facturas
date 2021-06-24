Three Dog
<template>
    <div>
        <b-button v-b-modal.modal-prevent-closing>Nuevo CFDI</b-button>

        <b-modal
        id="modal-prevent-closing"
        ref="modal"
        title="Nuevo CFDI"
        @show="resetModal"
        @hidden="resetModal"
        @ok="store"
        >
        <form ref="form" @submit.stop.prevent="handleSubmit">
            <b-form-group
            label="Tipo de documento"
            label-for="tipo_de_documento"
            invalid-feedback="Tipo de documento es requerido"
            >
                <b-form-input
                    id="tipo_de_documento"
                    v-model="cfdi.tipo_de_documento"
                    required
                ></b-form-input>
            </b-form-group>

            <b-form-group
            label="Folio"
            label-for="folio"
            invalid-feedback="Folio es requerido"
            >
                <b-form-input
                    id="folio"
                    v-model="cfdi.folio"
                    required
                ></b-form-input>
            </b-form-group>

            <b-form-group
            label="Serie"
            label-for="serie"
            invalid-feedback="Serie es requerido"
            >
                <b-form-input
                    id="serie"
                    v-model="cfdi.serie"
                    required
                ></b-form-input>
            </b-form-group>

            <b-form-group
            label="Total"
            label-for="total"
            invalid-feedback="Total es requerido"
            >
                <b-form-input
                    id="total"
                    type="number"
                    v-model="cfdi.total" 
                    required
                ></b-form-input>
            </b-form-group>

            <b-form-group
            label="Fecha"
            label-for="fecha"
            invalid-feedback="Fecha es requerido" 
            >
                <b-form-input
                    id="total"
                    type="date"
                    v-model="cfdi.fecha" 
                    required
                ></b-form-input>
            </b-form-group>

            <b-form-group
            label="Estatus"
            label-for="estatus"
            invalid-feedback="Estatus es requerido" 
            > 
                <b-form-select v-model="cfdi.estatus" :options="options"></b-form-select>
            </b-form-group>

        </form>
        </b-modal>

        <b-table
            id="table-transition-example"
            responsive
            :items="items"
            :fields="fields"
            striped
            small
            primary-key="a"
            :tbody-transition-props="transProps"
            caption-top
            class="mt-2"
        >
            <template #cell(opciones)="row">
                <b-button size="sm" class="mr-2" variant="danger" @click="cancel()">
                    Cancelar CFDI
                </b-button>
                <b-button size="sm" class="mr-2" variant="info">
                    Email CFDI
                </b-button>
            </template>
        </b-table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            transProps: {
                // Transition name
                name: "flip-list"
            },
            cfdi: {
                tipo_de_documento: '',
                folio: '',
                serie: '',
                total: '',
                fecha: '',
                estatus: null,
            },
            items: [
                {
                    tipo_de_documento: "a",
                    folio: "a",
                    serie: "a",
                    total: "a",
                    fecha: "a",
                    estatus: "a",
                    opciones: "a"
                },
                {
                    tipo_de_documento: "b",
                    folio: "b",
                    serie: "b",
                    total: "b",
                    fecha: "b",
                    estatus: "b",
                    opciones: "b"
                },
                {
                    tipo_de_documento: "c",
                    folio: "c",
                    serie: "c",
                    total: "c",
                    fecha: "c",
                    estatus: "c",
                    opciones: "c"
                }
            ],
            fields: [
                { key: "tipo_de_documento", sortable: true },
                { key: "folio", sortable: true },
                { key: "serie", sortable: true },
                { key: "total", sortable: true },
                { key: "fecha", sortable: true },
                { key: "estatus", sortable: true },
                { key: "opciones", sortable: true }
            ],
            options: [
                {value: null, text: 'Selecciona una opción'},
                {value: 'activo', text: 'Activo'},
                {value: 'inactivo', text: 'Inactivo'},
            ]
        };
    },
    created() {
        // this.list();
    },
    methods: {
        list() {
            axios.get('https://devfactura.in/api/v3/cfdi33/list', {
                headers: {
                    'F-PLUGIN': '9d4095c8f7ed5785cb14c0e3b033eeb8252416ed',
                    'F-Api-Key': 'JDJ5JDEwJHNITDlpZ0ZwMzdyd0RCTzFHVXlUOS5XVnlvaFFjd3ZWcnRBZHBIV0Q5QU5xM1Jqc2lpNlVD',
                    'F-Secret-Key': 'JDJ5JDEwJHRXbFROTHNiYzRzTXBkRHNPUVA3WU83Y2hxTHdpZHltOFo5UEdoMXVoakNKWTl5aDQwdTFT'
                }
            })
            .then(response => {
                console.log(response);
            })
            .catch(error => {
                console.log(error);
            })
        },
        store() {
            axios.post('https://devfactura.in/api/v3/cfdi33/create', this.cfdi, {
                headers: {
                    'F-PLUGIN': '9d4095c8f7ed5785cb14c0e3b033eeb8252416ed',
                    'F-Api-Key': 'JDJ5JDEwJHNITDlpZ0ZwMzdyd0RCTzFHVXlUOS5XVnlvaFFjd3ZWcnRBZHBIV0Q5QU5xM1Jqc2lpNlVD',
                    'F-Secret-Key': 'JDJ5JDEwJHRXbFROTHNiYzRzTXBkRHNPUVA3WU83Y2hxTHdpZHltOFo5UEdoMXVoakNKWTl5aDQwdTFT'
                }
            })
            .then(response => {
                console.log(response);
                // this.list()
            })
            .catch(error => {
                console.log(error);
            })
        },
        cancel(id) {
            
            if(confirm("¿Estas segur@ de cancelar este CFDI?")) {

                axios.post('https://devfactura.in/api/v3/cfdi33/55c0fdc67593d/cancel', id, {
                    headers: {
                    'F-PLUGIN': '9d4095c8f7ed5785cb14c0e3b033eeb8252416ed',
                    'F-Api-Key': 'JDJ5JDEwJHNITDlpZ0ZwMzdyd0RCTzFHVXlUOS5XVnlvaFFjd3ZWcnRBZHBIV0Q5QU5xM1Jqc2lpNlVD',
                    'F-Secret-Key': 'JDJ5JDEwJHRXbFROTHNiYzRzTXBkRHNPUVA3WU83Y2hxTHdpZHltOFo5UEdoMXVoakNKWTl5aDQwdTFT'
                    }
                })
                .then(response => {
                    console.log(response);
                    // this.list()
                })
                .catch(error => {
                    console.log(error);
                })
            }
        },
        sendEmail(id) {
                axios.post('https://devfactura.in/api/v3/cfdi33/55c0fdc67593d/email', id, {
                    headers: {
                    'F-PLUGIN': '9d4095c8f7ed5785cb14c0e3b033eeb8252416ed',
                    'F-Api-Key': 'JDJ5JDEwJHNITDlpZ0ZwMzdyd0RCTzFHVXlUOS5XVnlvaFFjd3ZWcnRBZHBIV0Q5QU5xM1Jqc2lpNlVD',
                    'F-Secret-Key': 'JDJ5JDEwJHRXbFROTHNiYzRzTXBkRHNPUVA3WU83Y2hxTHdpZHltOFo5UEdoMXVoakNKWTl5aDQwdTFT'
                    }
                })
                .then(response => {
                    console.log(response);
                    alert("Email enviado exitosamente!");
                })
                .catch(error => {
                    console.log(error);
                })

        },
        resetModal() {
            this.cfdi.tipo_de_documento = '';
            this.cfdi.folio = '';
            this.cfdi.serie = '';
            this.cfdi.total = '';
            this.cfdi.fecha = '';
            this.cfdi.status = '';
        }
    }
};
</script>

<style>
table#table-transition-example .flip-list-move {
    transition: transform 1s;
}
</style>
