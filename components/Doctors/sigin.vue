<template>
  <v-container fluid>
    <v-container>
      <v-card-text>
        <div class="posicion">
          <div class="login-box">
            <h2>Iniciar sesión</h2>
            <v-form ref="formLogin">
              <v-card-text>
                <div class="user-box">
                  <v-text-field
                    v-model="editedItem.name"
                    label="Nombre"
                    required
                    :rules="[v => !!v || 'Este campo es obligatorio']"
                  ></v-text-field>
                </div>

                <div class="user-box">
                  <v-text-field
                    v-model="editedItem.app"
                    label="Apellido Paterno"
                    required
                    :rules="[v => !!v || 'Este campo es obligatorio']"
                  ></v-text-field>
                </div>

                <div class="user-box">
                  <v-text-field
                    v-model="editedItem.apm"
                    label="Apellido Materno"
                    required
                    :rules="[v => !!v || 'Este campo es obligatorio']"
                  ></v-text-field>
                </div>

                <div class="user-box">
                  <v-text-field
                    v-model="editedItem.email"
                    label="Email"
                    required
                    :rules="[
                      v => !!v || 'Este campo es obligatorio',
                      v => /.+@.+\..+/.test(v) || 'El correo electrónico no es válido',
                      v => !duplicatedEmails.includes(v) || 'Este correo ya está registrado'
                    ]"
                  ></v-text-field>
                </div>

                <div class="user-box">
                  <v-text-field
                    v-model="editedItem.password"
                    label="Contraseña"
                    type="password"
                    id="password"
                    required
                    :rules="[
                      v => !!v || 'Este campo es obligatorio',
                      v => v && v.length >= 9 || 'La contraseña debe tener al menos 9 caracteres'
                    ]"
                  ></v-text-field>
                </div>

                <div class="user-box">
                  <v-text-field
                    v-model="editedItem.phone"
                    label="Teléfono"
                    required
                    :rules="[v => !!v || 'Este campo es obligatorio']"
                  ></v-text-field>
                </div>

                <div class="row" id="groupbtn">
                  <div>
                    <v-btn class="btn" @click="registrarSistema">
                      <span></span>
                      <span></span>
                      <span></span>
                      <span></span>
                      Registrar
                    </v-btn>
                  </div>
                  <div>
                    <v-btn class="btn" @click="regresarLogin">
                      <span></span>
                      <span></span>
                      <span></span>
                      <span></span>
                      regresa
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
    duplicatedEmails: [] // Arreglo para almacenar correos duplicados
  }),
  methods: {
    regresarLogin() {
      this.$router.push('/');
    },
    async registrarSistema() {
      try {
        // Verificar si el correo ya está en la lista de correos duplicados
        if (this.duplicatedEmails.includes(this.editedItem.email)) {
          // Mostrar un mensaje o manejar el error como desees
          console.error('Este correo ya está registrado');
          return;
        }

        // Realizar una solicitud POST al servidor backend
        const response = await this.$axios.post('http://localhost:5000/registerDoc', {
          name: this.editedItem.name,
          app: this.editedItem.app,
          apm: this.editedItem.apm,
          email: this.editedItem.email,
          password: this.editedItem.password,
          phone: this.editedItem.phone,
          // Otros campos del formulario
        });

        // Actualizar la lista de correos duplicados si la solicitud es exitosa
        this.duplicatedEmails.push(this.editedItem.email);

        // Puedes hacer algo con la respuesta del backend, como mostrar un mensaje de éxito
        console.log('Respuesta del backend:', response.data);
        
      } catch (error) {
        console.error('Error al registrar el doctor:', error);
      }
    }
    // ... (Nuevos métodos)
  }
};
</script>

<style lang="scss" scoped>
@import '../../assets/css/sigin.css';
</style>