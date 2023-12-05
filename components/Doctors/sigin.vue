<template>
    <v-container fluid>
        <v-container>
            <v-card-text>
                <div class="posicion">
                    <div class="login-box">
                        <h2>Sig In</h2>
                        <v-form ref="formLogin">
                            <v-card-text>
                                <div class="user-box">

                                    <input v-model="editedItem.name" required="">
                                    <label>Nombre</label>

                                </div>

                                <div class="user-box">

                                    <input v-model="editedItem.app" required="">
                                    <label>Apellido Paterno</label>

                                </div>

                                <div class="user-box">

                                    <input v-model="editedItem.apm" required="">
                                    <label>Apellido Materno</label>

                                </div>

                                <div class="user-box">

                                    <input v-model="editedItem.email" required="">
                                    <label>Email</label>

                                </div>

                                <div class="user-box">

                                    <input v-model="editedItem.password" type="password" id="password" required="">
                                    <label>Password</label>

                                </div>

                                <div class="user-box">

                                    <input v-model="editedItem.phone" required="">
                                    <label>Phone</label>

                                </div>
                                <div class="row" id="groupbtn">
                                    <div>
                                        <v-btn class="btn" @click="registrarSistema">
                                        <span></span>
                                        <span></span>
                                        <span></span>
                                        <span></span>
                                            Submit
                                        </v-btn>
                                    </div>
                                    <div>
                                        <v-btn class="btn" @click="regresarLogin">
                                        <span></span>
                                        <span></span>
                                        <span></span>
                                        <span></span>
                                            Go Back
                                    </v-btn>
                                </div>
                                </div>
                            </v-card-text>
                        </v-form>
                    </div>
                </div>
            </v-card-text>
        </v-container>
    </v-container>
</template>

<script>
export default {
    data: () => ({
        editedItem: {
                name: '',
                app: '',
                apm: '',
                email: '',
                password: '',
                phone: ''
        },
    }),
    methods: {
        regresarLogin(){
            this.$router.push('/')
        },
        async registrarSistema () {
            try {
         // Realizar una solicitud POST al servidor backend
        const response = await this.$axios.post('http://localhost:5000/registerDoc', {
            name: this.editedItem.name,
            app: this.editedItem.app,
            apm: this.editedItem.apm,
            email: this.editedItem.email,
            password: this.editedItem.password,
            phone: this.editedItem.phone,
           // Otros campos del formulario
        }
        );
         // Puedes hacer algo con la respuesta del backend, como mostrar un mensaje de éxito
        console.log('Respuesta del backend:', response.data);
        } catch (error) {
        console.error('Error al registrar el doctor:', error);
        }
        },
    }
};
</script>

<style lang="scss" scoped>
    @import '../../assets/css/sigin.css'
</style>