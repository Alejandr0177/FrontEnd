<template>
    <div class="cuerpo" style="background-color: #333; color: #fff; height: 890px;">
        <div class="header" style="display: flex; background: #555; height: 78px;">
            <div class="ubicacion" style="display: flex; margin-top: 20px; margin-left: 10px;">
                <p style="color: #fff; font-family: Open Sans; font-size: 18px; font-style: normal; font-weight: 700; line-height: 25px; letter-spacing: 0.36px;">Doctor ></p>
                &nbsp;<p>Schedule</p>
            </div>
            <div class="notificaciones" style="display: flex;">
                <v-icon class="bi bi-bell-fill" style="display: flex; color: #000; margin-top: -10px;"></v-icon>&nbsp;&nbsp;&nbsp;&nbsp;
                <v-btn style="width: 140px; height: 45px; flex-shrink: 0; border-radius: 23px; background: #4FB783; margin-top: 15px;">
                    Available
                </v-btn>
            </div>
        </div>
        <div style="display: flex;">
            <div>
                <div class="calendario" style="width: 705px; height: auto; flex-shrink: 0; border-radius: 15px; background: #444; margin-top: 20px; margin-left: 20px;">
                    <div style="width: 680px; margin-left: 10px; margin-top: 20px;">
                        <v-calendar v-model="selectedDate" :events="eventos">
                            <v-date-picker mode="single"></v-date-picker>
                        </v-calendar>
                    </div>
                    <div v-if="selectedDate" class="citasDia">
                        <h2 style="text-align: center">Citas para {{ selectedDate }}</h2>
                        <ul style="text-align: center;">
                            <li v-for="cita in citas" :key="cita.id">
                                {{ cita.date }} - {{ cita.time }}: {{ cita.motivo }}
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="lineaTiempo" style="width: 705px; height: auto; flex-shrink: 0; border-radius: 15px; background: #444; margin-top: 20px; margin-left: 20px;">
                    <div style="padding: 10px">
                        <v-timeline>
                            <v-timeline-item v-for="(dia, index) in dias" :key="index">
                                <div :style="{ backgroundColor: getCitaColor(index) }" style="border-radius: 10px; height: auto;">
                                    <p>{{ dia.date }} - {{ dia.time }}</p>
                                    <p>{{ dia.motivo }}</p>
                                </div>
                            </v-timeline-item>
                        </v-timeline>
                    </div>
                </div>
            </div>
            <div class="toDo" style="width: 425px; height: 760px; flex-shrink: 0; border-radius: 15px; background: #444; margin-left: 20px; margin-top: 20px;">
                <div style="margin-top: 20px; margin-left: 15px;">
                    <p>Todo List</p>
                </div>
                <div style="display: flex; margin-top: 15px; margin-left: 15px;">
                    <div style="width: 250px;">
                        <v-text-field solo v-model="nuevaTarea" @keyup.enter="agregarTarea" placeholder="Nueva tarea"></v-text-field>
                    </div>
                    <div style="margin-left: 20px; margin-top: 20px;">
                        <button @click="agregarTarea">Agregar</button>
                    </div>
                </div>
                <ul>
                    <li v-for="(tarea, index) in tareas" :key="index" :style="{ textDecoration: tarea.hecho ? 'line-through' : 'none' }">
                        <input type="checkbox" v-model="tarea.hecho" />
                        {{ tarea.texto }}
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
const coloresPersonalizados = ['#E3F2FD', '#FFF9C4', '#FFCDD2', '#E8F5E9', '#E1BEE7']
export default{
    data() {
        return {
            selectedDate: null,
            citas: [],
            nuevaTarea: "",
            tareas: [],
            eventos: [],
            dias: []
        }
    },
    mounted(){
        this.fetchCitas()
        this.fetchLinea()
    },
    methods: {
        agregarTarea() {
            if (this.nuevaTarea.trim() !== "") {
                this.tareas.push({ texto: this.nuevaTarea, hecho: false });
                this.nuevaTarea = "";
            }
        },
        async fetchCitas() {
            try {
                const response = await fetch('http://localhost:5000/get-calendario');
                const data = await response.json();
                if (data.alert === 'success') {
                    this.citas = data.citas.filter(cita => {
                        return cita.date === this.selectedDate
                    })

                    this.eventos = data.citas.map((cita) => ({
                        start: cita.date,
                        end: cita.date,
                        color: 'green',
                    }));
                } else {
                    console.error('Error al obtener citas:', data.error);
                }
            } catch (error) {
                console.error('Error de red:', error);
            }
        },
        async fetchLinea() {
            try {
                const response = await fetch('http://localhost:5000/get-dias');
                const data = await response.json();
                if (data.alert === 'success') {
                    this.dias = data.citas.sort((a, b) => a.date.localeCompare(b.date));
                } else {
                    console.error('Error al obtener citas:', data.error);
                }
            } catch (error) {
                console.error('Error de red:', error);
            }
        },
        getCitaColor(index) {
            const colorIndex = index % coloresPersonalizados.length;
            return coloresPersonalizados[colorIndex];
        },
    },
    watch: {
        selectedDate: 'fetchCitas',
    }
}
</script>

<style>
 /*
Calendario terminado 
*/
</style>
