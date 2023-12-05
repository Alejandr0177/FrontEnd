<template>
    <v-container fluid>
        <v-container>
            <div class="posicion">
                <div class="login-box">
                    <h2>Login</h2>
                    <v-form v-model="isValid" ref="formLogin">

                    <div class="user-box">

                        <input v-model="usuario" required="" :rules="[regla]">
                        <label>Email</label>

                    </div>

                    <div class="user-box">

                        <input type="password" id="password" name="ingresoPassword" required="">
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
    data () {
        return {
            usuario: '',
            password: '',
            isValid: false,
            regla: {
                vacio: value => !!value || 'Campo Requerido',
                cantidad: value => value.length > 8 || 'Minimo 8 caracteres'
            }
        }
    }, 
    methods: {
        ingresarSistema () {
            const valid = this.$refs.formLogin.validate()
            if(valid) {
                //cambiar pagina
                this.$router.push('/dashboard')
            }
            else {
                //alerta
                alert('Datos incorrectos')
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