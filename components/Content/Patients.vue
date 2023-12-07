<template>
    <v-data-table
        :headers="headers"
        :items="desserts"
        :search="search"
        class="elevation-1"
        >
        <template v-slot:top>
            <v-toolbar flat> 
                <v-toolbar-title>Patietns</v-toolbar-title>
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
                    <template v-slot:activator="{ on, attrs } ">
                        <v-btn
                            color="primary"
                            dark
                            class="mb-2"
                            v-bind="attrs"
                            v-on="on"
                            @click="openNewDialog"
                            >
                            New Item
                        </v-btn>
                    </template>
                    <v-card>
                        <v-card-title><span class="text-h5">{{ formTitle }}</span></v-card-title>
                        <v-card-text>
                            <v-container>
                                <!-- <v-row v-if="!isNewItem">

                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                    >
                                        <v-text-field
                                        v-model="editedItem.id"
                                        label="Patient ID"
                                        type=""
                                        :readonly="isReadOnly"
                                        ></v-text-field>
                                    </v-col>
                                </v-row> -->
                                <v-row>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-text-field
                                        v-model="editedItem.pat_name"
                                        label="Name"
                                        >
                                        </v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-text-field
                                        v-model="editedItem.pat_lastname"
                                        label="Last Name"
                                        >
                                        </v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-text-field
                                            v-model="editedItem.pat_email"
                                            label="Email"
                                            
                                            >
                                        </v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-text-field
                                            v-model="editedItem.pat_phone"
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
                                                v-model="editedItem.pat_birth"
                                                label="Date of Birth"
                                                prepend-icon="mdi-calendar"
                                                v-bind="attrs"
                                                v-on="on"
                                            ></v-text-field>
                                            </template>
                                            <v-date-picker
                                                v-model="editedItem.pat_birth"
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
                                        v-model="editedItem.pat_gender"
                                        :items="selgender"
                                        label="Gender"
                                        ></v-select>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-text-field
                                            v-model="editedItem.pat_treatment"
                                            label="Treatment"
                                            >
                                        </v-text-field>
                                    </v-col>
                                    <v-col
                                        cols="12"
                                        sm="6"
                                        md="6"
                                        >
                                        <v-text-field
                                            v-model="editedItem.pat_bloodgroup"
                                            label="Blood Group"
                                            >
                                        </v-text-field>
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
    </v-data-table>
</template>

<script>
    export default {
        data: () => ({
            constantIdDoc: '',
            dialog: false,
            dialogDelete: false,
            isNewItem: false,
            isReadOnly: true,
            //date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
            calendBirth: false,
            search: '',
            headers: [
                { text: 'Name', align: 'start', sortable: false, value: 'pat_name'},
                { text: 'Phone', value: 'pat_phone', sortable: false },
                { text: 'Email', value: 'pat_email', sortable: false },
                { text: 'Birth', value: 'pat_birth', sortable: false },
                { text: 'Gender', value: 'pat_gender', sortable: false  },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
            selgender: ['Male', 'Female', 'Other'],
            desserts: [],
            editedIndex: -1,
            editedItem: {
                id_pat: '',
                id_doc: '',
                pat_name: '',
                pat_lastname: '',
                pat_email: '',
                pat_phone: '',
                pat_birth: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().split('T')[0],   
                pat_gender: '',
                pat_treatment: '',
                pat_bloodgroup: '',
            },
            defaultItem: {
                id_pat: '',
                id_doc: '',
                pat_name: '',
                pat_lastname: '',
                pat_email: '',
                pat_phone: '',
                pat_birth: '',
                pat_gender: '',
                pat_treatment: '',
                pat_bloodgroup: '',
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
        },

        methods: {
            async initialize () {
                const patients_clin = await fetch('http://localhost:5000/showPats');
                const res = await patients_clin.json();
                console.log(res)
                if (res.alert === 'success') {
                    this.desserts = res.data.map(patient => {
                    return {
                    ...patient,
                    pat_birth: patient.pat_birth.split('T')[0]  
                        };
                    });
                }
                console.log('@@@ pacientes => ', this.desserts);
            },

            editItem(item) {
                this.isNewItem = false; 
                this.isReadOnly = true; 
                this.editedIndex = this.desserts.indexOf(item);
                this.editedItem = Object.assign({}, item);
                this.editedItem.id_pat = item.id_pat;
                this.dialog = true; 
            },
            openNewDialog() {
                this.isNewItem = true; 
                this.isReadOnly = false; 
                this.editedItem.id_doc = this.constantIdDoc;
                this.dialog = true; 
            },

            deleteItem (item) {
                this.editedIndex = this.desserts.indexOf(item)
                this.editedItem = Object.assign({}, item)
                this.dialogDelete = true
                this.editedItem.id_pat = item.pat_id;
            },

            async deleteItemConfirm () {
                try {
                if (!this.editedItem.id_pat) {
                    console.error('No se proporcionó el ID del paciente.');
                    return;
                }
                console.log(this.editedItem.id)
                const response = await this.$axios.delete(`http://localhost:5000/deletePat/${this.editedItem.id_pat}`);
                console.log('Respuesta del backend:', response.data);
                this.isNewItem = false;

                if (response.data.error === null) {
                    this.desserts.splice(this.editedIndex, 1);
                }

                this.closeDelete();
                } catch (error) {
                console.error('Error al eliminar el paciente:', error);
                }
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
            save() {
                if (this.isNewItem) {
                    this.registrarPaciente(); 
                } else {
                    this.actualizarPaciente(); 
                }
                this.close();

                this.initialize();
            },
            async registrarPaciente() {
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
            async actualizarPaciente() {
                try {
                    if (!this.editedItem.pat_id) {
                    console.error('No se proporcionó el ID del paciente.');
                    return;
                    }

                    // Realizar una solicitud POST al servidor backend
                    const response = await this.$axios.put(`http://localhost:5000/updatePat`, {
                    pat_id: this.editedItem.pat_id,
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

                    this.close();
                    this.initialize(); 
                    } catch (error) {
                    console.error('Error al actualizar el paciente:', error);
                }
            },
        }   
    }
</script>