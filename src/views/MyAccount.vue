<template>
  <v-app>
    <Navbar></Navbar>
    <v-row align="center justify-center">
      <v-col cols="auto">
        <v-hover v-slot="{ hover }" close-delay="120">
          <v-card
            class="register-card"
            rounded="xl"
            :elevation="hover ? 16 : 6"
            :class="{ 'on-hover': hover }"
          >
            <v-card-title class="justify-center">
              <v-avatar color="primary" size="180">
                <v-icon color="#ffff" size="200">mdi-account-circle</v-icon>
              </v-avatar>
            </v-card-title>
            <v-card-subtitle>
              <br />
              <p class="text-h4">
                {{ user.displayName }}
                <v-btn
                  class="icon-edit"
                  elevation="0"
                  plain
                  color="primary"
                  small
                  fab
                  to="./ModifyInformation"
                >
                  <v-icon>mdi-account-edit</v-icon></v-btn
                >
              </p>
            </v-card-subtitle>
            <v-card-text>
              <v-form class="mt-6" @submit.prevent="register">
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" class="col-register">
                      <h2>RUC</h2>
                      <br />
                      <p>{{ ruc }}</p>
                    </v-col>
                    <v-col cols="12" sm="6" class="col-register">
                      <h2>Nombre</h2>
                      <br />
                      <p>{{ user.displayName }}</p>
                    </v-col>
                    <v-col cols="12" sm="6" class="col-register">
                      <br />
                      <h2>Email</h2>
                      <br />
                      <p>{{ user.email }}</p>
                    </v-col>
                    <v-col cols="12" sm="6" class="col-register">
                      <br />
                      <h2>Celular</h2>
                      <br />
                      <p>{{ celular }}</p>
                    </v-col>
                  </v-row>

                  <div class="text-center mt-12">
                    <v-btn
                      rounded
                      x-large
                      outlined
                      color="primary"
                      type="submit"
                      to="./ChangePassword"
                      >Cambiar contraseña</v-btn
                    >
                  </div>
                </v-container>
              </v-form>
            </v-card-text>
          </v-card>
        </v-hover>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import Navbar from '../components/Navbar.vue';
import '../firebase/init';
import firebase from 'firebase/compat/app';

let db = firebase.database();
let usersRef = db.ref('users');
export default {
  name: 'MyAccount',
  firebase: {
    users: usersRef,
  },
  data: () => ({
    user: null,
    ruc: '',
    celular: '',
  }),
  components: {
    Navbar,
  },
  created() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.user = user;
        usersRef.get().then((snapshot) => {
          snapshot.forEach((doc) => {
            const userx = doc.val();
            if (userx.email == this.user.email) {
              this.celular = userx.celular;
              this.ruc = userx.ruc;
              console.log('te encontré!');
            }
          });
        });
      } else {
        this.user = null;
      }
    });
  },
};
</script>

<style scoped>
.register-card {
  margin: auto;
  margin-top: 80px;
  width: 100vh;
  text-align: center;
  padding: 5%;
}

.icon-edit {
  position: absolute;
}
.col-register {
  padding-block: 0px;
}

.inputPrice input[type='number'] {
  -moz-appearance: textfield;
}
.inputPrice input::-webkit-outer-spin-button,
.inputPrice input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
</style>
