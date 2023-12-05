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
            dialog: false,
            dialogDelete: false,
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
                pat_name: '',
                pat_lastname: '',
                pat_email: '',
                pat_phone: '',
                pat_birth: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
                pat_gender: '',
                pat_treatment: '',
                pat_bloodgroup: '',
            },
            defaultItem: {
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
            //this.desserts = []
            // this.desserts = [
            // {
            //     name: 'Alejandro',
            //     phone: '4623292256',
            //     email: 'a.garciagarcia7@ugto.mx',
            //     age: 21,
            //     treatment: 'Belleza',
            // },
            // {
            //     name: 'Fran',
            //     phone: '4623292256',
            //     email: 'a.garciagarcia7@ugto.mx',
            //     age: 21,
            //     treatment: 'Belleza',
            // },
            // {
            //     name: 'Jesus',
            //     phone: '4623292256',
            //     email: 'a.garciagarcia7@ugto.mx',
            //     age: 21,
            //     treatment: 'Belleza',
            // },
            // {
            //     name: 'Yair',
            //     phone: '4623292256',
            //     email: 'a.garciagarcia7@ugto.mx',
            //     age: 21,
            //     treatment: 'Belleza',
            // },
            // {
            //     name: 'Brandon',
            //     phone: '4623292256',
            //     email: 'a.garciagarcia7@ugto.mx',
            //     age: 21,
            //     treatment: 'Belleza',
            // },
            // ]
            const patients_clin = await fetch('http://localhost:5000/showPats')
            const res = await patients_clin.json()
            if (res.alert === 'success') {
                this.desserts = res.data
            }
            console.log('@@@ pacientes => ', this.desserts)
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
        },
    }
</script>