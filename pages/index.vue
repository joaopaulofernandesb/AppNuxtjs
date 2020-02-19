<template>
  <div>
    <v-app id="inspire">
      <v-navigation-drawer v-model="drawer" app clipped>
        <v-list dense>
          <v-list-item link>
            <v-list-item-action>
              <v-icon>mdi-view-dashboard</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Dashboard</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-list-item link to="/upload">
            <v-list-item-action>
              <v-icon>mdi-settings</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>Upload</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-app-bar app clipped-left>
        <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
        <v-toolbar-title>Aramazena Comprovante</v-toolbar-title>
      </v-app-bar>

      <v-content>
        <v-container>
          <v-card>
            <v-container fluid>
              <v-row>
                <v-col v-for="imagem in img" :key="imagem.id" class="d-flex child-flex" cols="4">
                  <v-card flat tile class="d-flex">
                    <v-img :src="'https://apicomprovate.herokuapp.com/images/'+imagem.filename">
                      <template v-slot:placeholder>
                        <v-row class="fill-height ma-0" align="center" justify="center">
                          <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                        </v-row>
                      </template>
                    </v-img>
                  </v-card>
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-container>
      </v-content>
    </v-app>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  props: {
    source: String
  },
  data: () => ({
    img: '',
    drawer: false
  }),
  created() {
    this.$vuetify.theme.dark = false
  },
  mounted() {
    setInterval(() => {
      axios
        .get('https://apicomprovate.herokuapp.com/api/img')
        .then(response => {
          console.log(response.data)
          this.img = response.data
        })
    }, 3500)
  }
}
</script>