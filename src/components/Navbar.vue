<template>
  <v-card class="mx-auto overflow-hidden">
    <v-app-bar light fixed>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>
        <router-link to="./"
          ><span>Diswallet</span></router-link
        ></v-toolbar-title
      >
      <v-spacer></v-spacer>
      <template>
        <div class="item-menu">
          Hola, <b>{{ user.displayName || user.email }}</b> !
        </div>
      </template>

      <div class="item-menu">
        <v-btn fab small color="primary" outlined @click.prevent="logout">
          <v-avatar>
            <v-icon color="primary">mdi-logout</v-icon>
          </v-avatar>
        </v-btn>
      </div>
    </v-app-bar>
    <v-navigation-drawer app v-model="drawer" temporary>
      <v-list nav dense>
        <v-list-item>
          <v-layout column align-start> Menu </v-layout>
          <v-layout column align-end>
            <v-app-bar-nav-icon @click.stop="drawer = !drawer"
              ><v-icon color="primary">mdi-menu</v-icon></v-app-bar-nav-icon
            >
          </v-layout>
        </v-list-item>
        <br />
        <v-list-item-group active-class="deep-purple--text text--accent-4">
          <router-link to="./">
            <v-list-item>
              <v-list-item-title>Inicio</v-list-item-title>
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>
            </v-list-item>
          </router-link>

          <v-list-group no-action>
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Descuentos</v-list-item-title>
              </v-list-item-content>
            </template>

            <v-list-item to="./bills">
              <v-list-item-title>Dscto. de Facturas</v-list-item-title>
              <v-list-item-icon>
                <v-icon>mdi-cash-fast</v-icon>
              </v-list-item-icon>
            </v-list-item>

            <v-list-item disabled>
              <v-list-item-title>Dscto. de Letras</v-list-item-title>
              <v-list-item-icon>
                <v-icon disabled>mdi-file-outline</v-icon>
              </v-list-item-icon>
            </v-list-item>

            <v-list-item disabled>
              <v-list-item-title>Dscto. de Recibos</v-list-item-title>
              <v-list-item-icon>
                <v-icon disabled>mdi-card-bulleted-outline</v-icon>
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>

          <router-link to="./myaccount">
            <v-list-item>
              <v-list-item-title>Mi Cuenta</v-list-item-title>
              <v-list-item-icon>
                <v-icon>mdi-account</v-icon>
              </v-list-item-icon>
            </v-list-item>
          </router-link>

          <v-list-item @click.prevent="logout">
            <v-list-item-title>Cerrar Sesión</v-list-item-title>
            <v-list-item-icon>
              <v-icon>mdi-logout</v-icon>
            </v-list-item-icon>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
  </v-card>
</template>

<script>
import '../firebase/init';
import firebase from 'firebase/compat/app';
export default {
  name: 'Navbar',
  data: () => ({
    drawer: false,
    group: null,
    user: null,
    menu: 'Diswallet',
  }),
  methods: {
    logout() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.push({ name: 'Login' });
        });
    },
  },
  created() {
    firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.user = user;
      } else {
        this.user = null;
      }
    });
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
}

.item-menu {
  padding: 2%;
}

.parallax {
  margin-top: 0px;
  background: rgb(126, 41, 205);
  background: linear-gradient(
    90deg,
    rgba(126, 41, 205, 1) 46%,
    rgba(56, 129, 199, 1) 100%
  );
}
</style>
