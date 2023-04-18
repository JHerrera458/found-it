<template>
  <div class="content">
    <v-row align="center">
      <v-col cols="6" align="right">
        <v-img src="/lostfound.png" max-width="400px" max-height="400px" />
      </v-col>
      <v-col cols="6" align="left">
        <v-card class="customCard" color="white" light>
          <v-card-title primary-title>
            <h1>Inicia sesión</h1>
          </v-card-title>
          <v-card-text>
            <v-form v-model="formLoginAccount" ref="formLoginAccount"></v-form>
            <v-text-field v-model="email" name="email" label="Correo electrónico" outlined prepend-icon="mdi-email" />
            <v-text-field @click:append="visiblePass = !visiblePass"
              :append-icon="visiblePass ? 'mdi-eye' : 'mdi-eye-off'" v-model="password"
              :type="visiblePass ? 'text' : 'password'" name="password" label="Contraseña" outlined
              prepend-icon="mdi-lock" />
          </v-card-text>
          <v-card-actions>
            <v-btn dark class="customBtn" color="#03396c" @click="loginAccount()" :loading="loading">Iniciar
              sesión</v-btn>
            <v-btn dark class="customBtn" color="#005b96" @click="dialog = !dialog">Crear una cuenta</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="dialog" max-width="500">
      <v-card color="white" light>
        <v-card-title>
          Crea una cuenta nueva
        </v-card-title>
        <v-card-text>
          <v-form v-model="formCreateAccount" ref="formCreateAccount">
            <v-text-field v-model="registerInfo.name" :rules="[rules.required]" name="name" label="Nombre" outlined
              prepend-icon="mdi-account" />
            <v-text-field v-model="registerInfo.lastName" :rules="[rules.required]" name="lastName" label="Apellido"
              outlined prepend-icon="mdi-account" />
            <v-text-field v-model="registerInfo.phoneNumber" :rules="[rules.required]" name="number"
              label="Número de celular" outlined prepend-icon="mdi-phone" />
            <v-text-field v-model="registerInfo.email" :rules="[rules.required, rules.email]" name="email"
              label="Correo electrónico" outlined prepend-icon="mdi-email" />
            <v-text-field @click:append="visiblePass = !visiblePass"
              :append-icon="visiblePass ? 'mdi-eye' : 'mdi-eye-off'" v-model="registerInfo.password"
              :type="visiblePass ? 'text' : 'password'" :rules="[rules.required]" name="password" label="Contraseña"
              outlined prepend-icon="mdi-lock" />
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn dark color="#005b96" @click="createAccount()" :loading="loading">Crear una cuenta</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  layout: 'blank',
  data() {
    return {
      accountExist: false,
      loading: false,
      formCreateAccount: null,
      formLoginAccount: null,
      visiblePass: false,
      dialog: false,
      email: "",
      password: "",
      registerInfo: {
        name: "",
        lastName: "",
        phoneNumber: "",
        email: "",
        password: "",
        role: 0,
        createDate: new Date()
      },
      rules: {
        required: (value) => !!value || 'Este campo es obligatorio',
        email: (value) => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || 'Ingresa un email válido'
        },
      }
    }
  },
  methods: {
    async validateAccountExistance() {
      const url = `${process.env.URL_DEV}/accounts`
      await this.$axios.get(url).then((response) => {
        const data = response.data
        const found = data.find(account => account.email == this.registerInfo.email)
        if (found) {
          this.accountExist = true
        } else {
          this.accountExist = false
        }
      }).catch((error) => {
        this.accountExist = true
      }).finally(() => {
      })
    },
    async createAccount() {
      const refFormCreateAccount = this.$refs['formCreateAccount']
      if (refFormCreateAccount) {
        const formIsValid = refFormCreateAccount.validate()
        if (formIsValid) {
          await this.validateAccountExistance()
          if (this.accountExist) {
            this.$swal.fire({
              icon: 'warning',
              title: 'Correo ya registrado'
            })
          } else {
            this.loading = true
            const url = `${process.env.URL_DEV}/accounts`
            this.$axios.post(url, this.registerInfo).then((response) => {
              this.$swal.fire(
                'Cuenta creada!',
                'Exitosamente!',
                'success'
              )
            }).catch((error) => {
              this.$swal.fire({
                icon: 'error',
                title: 'Ocurrió un error creando la cuenta'
              })
            }).finally(() => {
              this.loading = false
              this.dialog = false
            })
          }
        } else {
          this.$swal.fire({
            icon: 'warning',
            title: 'Faltan campos en el formulario.'
          })
        }
      }
    },
    loginAccount() {
      const url = `${process.env.URL_DEV}/accounts`
      this.loading = true
      this.$axios.get(url).then((response) => {
        const data = response.data
        const found = data.find(account => account.email == this.email && account.password == this.password)
        if (found) {
          this.$swal.fire({
            title: 'Iniciaste sesión!',
            text: 'Bienvenido.',
            icon: 'success',
          })
          this.$router.push('/user/home')
        } else {
          this.$swal.fire({
            title: 'Error!',
            text: 'Usuario y/o contraseña incorrectos.',
            icon: 'error',
          })
        }
      }).catch((error) => {
        this.$swal.fire({
          title: 'Error!',
          text: 'Ha ocurrido un error.',
          icon: 'error',
        })
      }).finally(() => {
        this.loading = false
      })
    }
  }
}
</script>

<style>
.content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(to bottom, #baeeff, #8393ff);
}

.customBtn {
  width: 200px;
}

.customCard {
  max-width: 425px;
}
</style>
