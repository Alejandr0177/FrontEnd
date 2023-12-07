<template>
  <v-container fluid>
    <v-container>
      <div class="posicion">
        <div class="login-box">
          <h2>Login</h2>
          <v-form ref="formLogin">
            <div class="user-box">
              <input v-model="editedItem.email" :rules="[editedItem.regla.vacio, editedItem.regla.cantidad]" required>
              <label>Email</label>
            </div>

            <div class="user-box">
              <input v-model="editedItem.password" type="password" id="password" :rules="[editedItem.regla.vacio, editedItem.regla.cantidad]" required>
              <label>Password</label>
            </div>

            <div class="row" id="groupbtn">
              <div>
                <v-btn class="btn" @click="ingresarSistema" :disabled="!validarFormulario">
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
          <v-dialog v-model="loginAlert" max-width="525px">
            <v-card>
              <v-card-title class="text-h5">Incorrect Email or Password</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red darken-1" text @click="closeDelete">Close</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </div>
      </div>
    </v-container>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    loginAlert: false,
    editedItem: {
      email: '',
      password: '',
      regla: {
        vacio: value => !!value || 'Campo Requerido',
        cantidad: value => value.length > 8 || 'Mínimo 8 caracteres'
      }
    },
  }),

  computed: {
    validarFormulario() {
      // Validar que ambos campos estén llenos
      return !!this.editedItem.email && !!this.editedItem.password;
    }
  },

  watch: {
    emailAlert(val) {
      val || this.closeDelete()
    }
  },

  methods: {
    closeDelete() {
      this.loginAlert = false
    },
    async ingresarSistema() {
      // Validar antes de enviar la solicitud
      if (!this.validarFormulario) {
        return;
      }

      try {
        // Realizar una solicitud POST al servidor backend
        const response = await this.$axios.post('http://localhost:5000/loginDoc', {
          email: this.editedItem.email,
          password: this.editedItem.password,
          // Otros campos del formulario
        });

        const docId = response.data.data[0].doc_id;
        console.log('Valor de doc_id:', docId);

        // Puedes hacer algo con la respuesta del backend, como mostrar un mensaje de éxito
        console.log('Respuesta del backend:', response.data);

        if (response.data.alert === 'SESSION_START()') {
          // Redirigir solo después de confirmar la sesión
          this.$router.push(`/dashboard?doc_id=${docId}`);
          this.$root.$emit('docIdObtained', docId);
        }
      } catch (error) {
        if (!error.response) {
          console.log('No server Response', error);
        } else if (error.response.status === 401) {
          console.log('Error al iniciar sesión', error);
          this.loginAlert = true;
        }
      }
    },
    pageRegistro() {
      this.$router.push(`/sigin`);
    }
  }
};
</script>

<style lang="scss" scoped>
  @import '../../assets/css/login.css'
</style>
