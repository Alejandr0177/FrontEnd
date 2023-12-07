<template>
    <v-data-table
        :headers="headers"
        :items="desserts"
        :search="search"
        class="elevation-1"
        >
        <template v-slot:top>
            <v-toolbar flat> 
                <v-toolbar-title>Appointments</v-toolbar-title>
                <v-divider 
                    class="mx-4"
                    inset
                    vertical
                    >
                </v-divider>
                <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search Patients"
                    single-line
                    hide-details
                    >
                </v-text-field>
                <v-spacer></v-spacer>
                <v-dialog
                    v-model="dialog"
                    max-width="500px"
                    >
                    <template v-slot:activator="{ on, attrs }">
                        <v-btn
                            color="primary"
                            dark
                            class="mb-2"
                            v-bind="attrs"
                            v-on="on"
                            >
                            New Item
                        </v-btn>
                    </template>
                    <v-card>
                        <v-card-title><span class="text-h5">{{ formTitle }}</span></v-card-title>
                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-select
                                        v-model="editedItem.name"
                                        @click="listaName()"
                                        :items="listaNames"
                                        label="Name"
                                        ></v-select>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="10"
                                        >
                                        <v-text-field
                                            v-model="editedItem.email"
                                            label="Email"
                                            >
                                        </v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="10"
                                        >
                                        <v-text-field
                                            v-model="editedItem.phone"
                                            label="Phone"
                                            >
                                        </v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-menu
                                            v-model="calendBirth"
                                            :close-on-content-click="false"
                                            :nudge-right="40"
                                            transition="scale-transition"
                                            offset-y
                                            min-width="auto"
                                        >
                                            <template v-slot:activator="{ on, attrs }">
                                            <v-text-field
                                                v-model="editedItem.date"
                                                label="Date"
                                                prepend-icon="mdi-calendar"
                                                v-bind="attrs"
                                                v-on="on"
                                            ></v-text-field>
                                            </template>
                                            <v-date-picker
                                                v-model="editedItem.date"
                                                @input="calendBirth = false"
                                                >
                                            </v-date-picker>
                                        </v-menu>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-select
                                        v-model="editedItem.time"
                                        @click="lista()"
                                        :items="datetime"
                                        label="Time"
                                        ></v-select>
                                    </v-col>
                                </v-row>
                            </v-container>
                        </v-card-text>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn
                                color="blue darken-1"
                                text
                                @click="close"
                                >
                                Cancel
                            </v-btn>
                            <v-btn
                                color="blue darken-1"
                                text
                                @click="save"
                                >
                                Save
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="525px">
                    <v-card>
                        <v-card-title class="text-h5">Are you sure you want to delete this patient?</v-card-title>
                        <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                        <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                        <v-spacer></v-spacer>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
            <v-icon
                class="mr-2"
                @click="editItem(item)"
                color="yellow"
            >
                mdi-pencil-box-outline
            </v-icon>
            <v-icon
                @click="deleteItem(item)"
                color="red"
            >
                mdi-trash-can-outline
            </v-icon>
        </template>
        <template v-slot:no-data>
            <v-btn
                color="primary"
                @click="initialize"
            >
                Reset
            </v-btn>
        </template>
    </v-data-table>
</template>

<script>
    export default {
        data: () => ({
            dialog: false,
            dialogDelete: false,
            //date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
            calendBirth: false,
            search: '',
            headers: [
                { text: 'Name', align: 'start', sortable: false, value: 'pat_nombre'},
                { text: 'Email', value: 'pat_email', sortable: false },
                { text: 'Phone', value: 'pat_phone', sortable: false },
                { text: 'Date', value: 'app_date', sortable: false },
                { text: 'Time', value: 'app_time', sortable: false  },
                { text: 'Status', value: 'status', sortable: false },
            ],
            datetime: [],
            lista(){
                for (let i = 9; i < 16; i++) {
                    let hrs
                    if (i <= 9) {
                        hrs = `0${i}`
                    } else{
                        hrs = i
                    }
                    for (let j = 0; j <= 45; j=j+15) {
                        let mts
                        if (j < 9) {
                            mts = `0${j}`
                            let time = `${hrs}:${mts}`
                            this.datetime.push(time)
                        } else{
                            let mts = j
                            let time = `${hrs}:${mts}`
                            this.datetime.push(time)
                        }
                    }
                }
                //console.log(this.datetime)
            },
            listaName() {
    // Clear existing names in the array
    this.listaNames = [];

    // Populate the array with patient names
    this.pacientes.forEach(patient => {
        // Check if pat_nombre and pat_apellido are defined
        if (patient.pat_name && patient.pat_lastname) {
            const fullName = `${patient.pat_name} ${patient.pat_lastname}`;
            this.listaNames.push(fullName);
        } else {
            console.error('Missing pat_nombre or pat_apellido in patient data:', patient);
        }
    });

    console.log('@@@ listaNames => ', this.listaNames);
},
            desserts: [],
            pacientes: [],
            editedIndex: -1,
            editedItem: {
                name: '',
                lastname: '',
                email: '',
                phone: '',
                birth: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
                gender: '',
                treatment: '',
                bloodgroup: '',
            },
            defaultItem: {
                name: '',
                lastname: '',
                email: '',
                phone: '',
                birth: '',
                gender: '',
                treatment: '',
                bloodgroup: '',
            },
        }),

        computed: {
        formTitle () {
            return this.editedIndex === -1 ? 'Patient' : 'Edit Item'
        },
        },

        watch: {
        dialog (val) {
            val || this.close()
        },
        dialogDelete (val) {
            val || this.closeDelete()
        },
        },

        created () {
            this.initialize()
            this.ObtenerPacientes();
        },

        methods: {
        initialize () {
            this.desserts = [
            //{
            //    name: 'Alejandro',
            //    phone: '4623292256',
            //    email: 'a.garciagarcia7@ugto.mx',
            //    age: 21,
            //    treatment: 'Belleza',
            //},
            //{
            //    name: 'Fran',
            //    phone: '4623292256',
            //    email: 'a.garciagarcia7@ugto.mx',
            //    age: 21,
            //    treatment: 'Belleza',
            //},
            //{
            //    name: 'Jesus',
            //    phone: '4623292256',
            //    email: 'a.garciagarcia7@ugto.mx',
            //    age: 21,
            //    treatment: 'Belleza',
            //},
            //{
            //    name: 'Yair',
            //    phone: '4623292256',
            //    email: 'a.garciagarcia7@ugto.mx',
            //    age: 21,
            //    treatment: 'Belleza',
            //},
            //{
            //    name: 'Brandon',
            //    phone: '4623292256',
            //    email: 'a.garciagarcia7@ugto.mx',
            //    age: 21,
            //    treatment: 'Belleza',
            //},
            ]
        },

        editItem (item) {
            this.editedIndex = this.desserts.indexOf(item)
            this.editedItem = Object.assign({}, item)
            this.dialog = true
        },

        deleteItem (item) {
            this.editedIndex = this.desserts.indexOf(item)
            this.editedItem = Object.assign({}, item)
            this.dialogDelete = true
        },

        deleteItemConfirm () {
            this.desserts.splice(this.editedIndex, 1)
            this.closeDelete()
        },

        close () {
            this.dialog = false
            this.$nextTick(() => {
            this.editedItem = Object.assign({}, this.defaultItem)
            this.editedIndex = -1
            })
        },

        closeDelete () {
            this.dialogDelete = false
            this.$nextTick(() => {
            this.editedItem = Object.assign({}, this.defaultItem)
            this.editedIndex = -1
            })
        },

        save () {
            if (this.editedIndex > -1) {
            Object.assign(this.desserts[this.editedIndex], this.editedItem)
            } else {
            this.desserts.push(this.editedItem)
            }
            this.close()
        },
        async initialize() {
        try {
            const response = await fetch('http://localhost:5000/showAppointments');
            const result = await response.json();

            if (result.alert === 'success') {
                this.desserts = result.data;
            } else {
                // Handle the case where the API call was successful but no data is returned
                this.desserts = [];
            }

            console.log('@@@ citas => ', this.desserts);
        } catch (error) {
            console.error('Error fetching data:', error);
            // Handle the error as needed
        }
    },
    async ObtenerPacientes() {
            try {
                const patients_clin = await fetch('http://localhost:5000/showPats');
                const res = await patients_clin.json();

                if (res.alert === 'success') {
                    this.pacientes = res.data.map(patient => {
                        return {
                            ...patient,
                            pat_birth: patient.pat_birth.split('T')[0],
                        };
                    });

                    // Populate the listaNames array when patients data is fetched
                    this.listaName();
                } else {
                    // Handle the case where the API call was successful but no data is returned
                    this.pacientes = [];
                }

                console.log('@@@ pacientes => ', this.pacientes);
            } catch (error) {
                console.error('Error fetching data:', error);
                // Handle the error as needed
            }
        },
        async registrarAppointments() {
                //const res 
                try {
                    const response = await this.$axios.post('http://localhost:5000/registerPat', {
                    name: this.editedItem.pat_name,
                    lastname: this.editedItem.pat_lastname,
                    email: this.editedItem.pat_email,
                    phone: this.editedItem.pat_phone,
                    birth: this.editedItem.pat_birth,
                    gender: this.editedItem.pat_gender,
                    treatment: this.editedItem.pat_treatment,
                    bloodgroup: this.editedItem.pat_bloodgroup,
                    doc_id: this.constantIdDoc,
                    });
                    console.log('Respuesta del backend:', response.data);
                    this.isNewItem = false;
                    this.close();
                    this.initialize(); 
                } catch (error) {
                    console.error('Error al registrar el paciente:', error);
                }
            },
    },
}
</script>