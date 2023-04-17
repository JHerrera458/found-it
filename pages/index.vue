<template>
  <v-container class="container">
    <v-row align="center">
      <v-col cols="6" align="right">
        <v-img src="/lostfound.png" max-width="400px" max-height="400px" />
      </v-col>
      <v-col cols="6" align="left">
        <v-card class="card" color="white" light>
          <v-card-title primary-title>
            <h1>Inicia sesión</h1>
          </v-card-title>
          <v-card-text>
            <v-text-field v-model="email" name="email" label="Correo electrónico" outlined prepend-icon="mdi-email" />
            <v-text-field @click:append="visiblePass = !visiblePass"
              :append-icon="visiblePass ? 'mdi-eye' : 'mdi-eye-off'" v-model="password"
              :type="visiblePass ? 'text' : 'password'" name="password" label="Contraseña" outlined
              prepend-icon="mdi-lock" />
          </v-card-text>
          <v-card-actions>
            <v-btn dark class="btn" color="#03396c">Iniciar sesión</v-btn>
            <v-btn dark class="btn" color="#005b96" @click="dialog = !dialog">Crear una cuenta</v-btn>
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
          <v-text-field v-model="register.name" name="name" label="Nombre" outlined prepend-icon="mdi-account" />
          <v-text-field v-model="register.lastName" name="lastName" label="Apellido" outlined
            prepend-icon="mdi-account" />
          <v-text-field v-model="register.phoneNumber" name="number" label="Número de celular" outlined
            prepend-icon="mdi-phone" />
          <v-text-field v-model="register.email" name="email" label="Correo electrónico" outlined
            prepend-icon="mdi-email" />
          <v-text-field @click:append="visiblePass = !visiblePass" :append-icon="visiblePass ? 'mdi-eye' : 'mdi-eye-off'"
            v-model="register.password" :type="visiblePass ? 'text' : 'password'" name="password" label="Contraseña"
            outlined prepend-icon="mdi-lock" />
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn dark color="#005b96">Crear una cuenta</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  layout: 'blank',
  data() {
    return {
      visiblePass: false,
      dialog: false,
      email: "",
      password: "",
      register: {
        name: "",
        lastName: "",
        phoneNumber: "",
        email: "",
        password: "",
      }
    }
  }
}
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(to bottom, #baeeff, #8393ff);
}

.btn {
  width: 200px;
}

.card {
  max-width: 425px;
}
</style>
