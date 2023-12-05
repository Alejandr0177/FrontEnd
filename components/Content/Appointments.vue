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
                                        md="10"
                                        >
                                        <v-text-field
                                        v-model="editedItem.name"
                                        label="Name"
                                        >
                                        </v-text-field>
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
                { text: 'Name', align: 'start', sortable: false, value: 'name'},
                { text: 'Email', value: 'email', sortable: false },
                { text: 'Phone', value: 'phone', sortable: false },
                { text: 'Date', value: 'date', sortable: false },
                { text: 'Time', value: 'time', sortable: false  },
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
            desserts: [],
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
        },

        methods: {
        initialize () {
            this.desserts = [
            {
                name: 'Alejandro',
                phone: '4623292256',
                email: 'a.garciagarcia7@ugto.mx',
                age: 21,
                treatment: 'Belleza',
            },
            {
                name: 'Fran',
                phone: '4623292256',
                email: 'a.garciagarcia7@ugto.mx',
                age: 21,
                treatment: 'Belleza',
            },
            {
                name: 'Jesus',
                phone: '4623292256',
                email: 'a.garciagarcia7@ugto.mx',
                age: 21,
                treatment: 'Belleza',
            },
            {
                name: 'Yair',
                phone: '4623292256',
                email: 'a.garciagarcia7@ugto.mx',
                age: 21,
                treatment: 'Belleza',
            },
            {
                name: 'Brandon',
                phone: '4623292256',
                email: 'a.garciagarcia7@ugto.mx',
                age: 21,
                treatment: 'Belleza',
            },
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
        },
    }
</script>