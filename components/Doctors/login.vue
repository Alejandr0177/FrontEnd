<template>
    <v-container fluid>
        <v-container>
            <div class="posicion">
                <div class="login-box">
                    <h2>Login</h2>
                    <v-form ref="formLogin">

                    
                        <div class="user-box">
                            <input v-model="editedItem.email" required="">
                            <label>Email</label>

                        </div>

                        <div class="user-box">

                        <input v-model="editedItem.password" type="password" id="password" required="" >
                        <label>Password</label>

                        </div>

                    <div class="row" id="groupbtn">
                        <div>
                            <v-btn class="btn" @click="ingresarSistema">
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                                Submit
                            </v-btn>
                        </div>
                        <div>
                            <v-btn class="btn" @click="pageRegistro">
                            <span></span>
                            <span></span>
                            <span></span>
                            <span></span>
                                Sig In
                            </v-btn>
                        </div>
                    </div>

                </v-form>
                </div>
            </div>
        </v-container>
    </v-container>
</template>

<script>
export default {
    data: () => ({
        editedItem: {
                email: '',
                password: '',
                regla: {
                vacio: value => !!value || 'Campo Requerido',
                cantidad: value => value.length > 8 || 'Minimo 8 caracteres'
            }
        },
    }),
    methods: {
        async ingresarSistema () {
            try {
         // Realizar una solicitud POST al servidor backend
        const response = await this.$axios.post('http://localhost:5000/loginDoc', {
            email: this.editedItem.email,
            password: this.editedItem.password,
           // Otros campos del formulario
        }
        );
         // Puedes hacer algo con la respuesta del backend, como mostrar un mensaje de éxito
        console.log('Respuesta del backend:', response.data.alert);
        if(response.data.alert === 'SESSION_START()'){
            this.$router.push('/dashboard')
        }
        } catch (error) {
        console.error('Error al iniciar sesion', error);
        }
        },
        pageRegistro () {
            this.$router.push('/Sigin')
        }
    }
};
</script>

<style lang="scss" scoped>
    @import '../../assets/css/login.css'
</style>