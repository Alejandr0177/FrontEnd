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

                    <v-btn class="btn" @click="ingresarSistema">
                        <span></span>
                        <span></span>
                        <span></span>
                        <span></span>
                        Submit
                    </v-btn>

                    

                    <v-btn class="btn" @click="pageRegistro">
                        <span></span>
                        <span></span>
                        <span></span>
                        <span></span>
                        Sig In
                    </v-btn>

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
                //this.$router.push('/dashboard')
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
.campoIncorrecto {
    background-color: aliceblue;
}

.posicion {
    display: flex;
    justify-content: center;
}

.login-box {
    top: 50%;
    width: 400px;
    padding: 40px;
    background: rgba(27, 28, 32, 0.5);
    box-sizing: border-box;
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.6);
    border-radius: 10px;

}

.login-box h2 {
    margin: 0 0 30px;
    padding: 0;
    color: #fff;
    text-align: center;
}

.login-box .user-box {
    position: relative;
}

.login-box .user-box input {
    width: 100%;
    padding: 10px 0;
    font-size: 16px;
    color: #fff;
    margin-bottom: 30px;
    border: none;
    border-bottom: 1px solid #fff;
    outline: none;
    background: transparent;
}

.login-box .user-box label {
    position: absolute;
    top: 0;
    left: 0;
    padding: 10px 0;
    font-size: 16px;
    color: #fff;
    pointer-events: none;
    transition: .5s;
}

.login-box .user-box input:focus~label,
.login-box .user-box input:valid~label {
    top: -20px;
    left: 0;
    color: aqua;
    font-size: 12px;
}

.btn {
    background-color: transparent;
}

.login-box form button {
    position: relative;
    display: inline-block;
    padding: 10px 20px;
    color: aqua;
    font-size: 16px;
    text-decoration: none;
    text-transform: uppercase;
    overflow: hidden;
    transition: .5s;
    margin-top: 40px;
    letter-spacing: 4px
}

.login-box button:hover {
    background: aqua;
    color: #fff;
    border-radius: 5px;
    box-shadow: 0 0 5px #03e9f4,
        0 0 25px #03e9f4,
        0 0 50px #03e9f4,
        0 0 100px #03e9f4;
}

.login-box button span {
    position: absolute;
    display: block;
}

.login-box button span:nth-child(1) {
    top: 0;
    left: -100%;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, transparent, #03e9f4);
    animation: btn-anim1 1s linear infinite;
}

@keyframes btn-anim1 {
    0% {
        left: -100%;
    }

    50%,
    100% {
        left: 100%;
    }
}

.login-box button span:nth-child(2) {
    top: -100%;
    right: 0;
    width: 2px;
    height: 100%;
    background: linear-gradient(180deg, transparent, #03e9f4);
    animation: btn-anim2 1s linear infinite;
    animation-delay: .25s
}

@keyframes btn-anim2 {
    0% {
        top: -100%;
    }

    50%,
    100% {
        top: 100%;
    }
}

.login-box button span:nth-child(3) {
    bottom: 0;
    right: -100%;
    width: 100%;
    height: 2px;
    background: linear-gradient(270deg, transparent, #03e9f4);
    animation: btn-anim3 1s linear infinite;
    animation-delay: .5s
}

@keyframes btn-anim3 {
    0% {
        right: -100%;
    }

    50%,
    100% {
        right: 100%;
    }
}

.login-box button span:nth-child(4) {
    bottom: -100%;
    left: 0;
    width: 2px;
    height: 100%;
    background: linear-gradient(360deg, transparent, #03e9f4);
    animation: btn-anim4 1s linear infinite;
    animation-delay: .75s
}

@keyframes btn-anim4 {
    0% {
        bottom: -100%;
    }

    50%,
    100% {
        bottom: 100%;
    }
}
</style>