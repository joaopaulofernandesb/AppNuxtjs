<template>
  <div>
    <v-app id="inspire">
      <v-navigation-drawer v-model="drawer" app clipped>
        <v-list dense>
          <v-list-item link to="/">
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
              <v-list-item-title>Settings</v-list-item-title>
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
          <v-alert
            :value="alert"
            color="green"
            dark
            border="top"
            icon="mdi-check"
            transition="scale-transition"
          >Concluido com Sucesso!</v-alert>
          <v-row align="center" justify="center">
            <v-col>
              <div v-for="imagem in img" :key="imagem.id">
                <center>
                  <v-img :src="'https://apicomprovate.herokuapp.com/images/'+imagem.filename"></v-img>
                </center>
              </div>
              <v-file-input
                accept="image/png, image/jpeg, image/bmp"
                v-model="file"
                outlined
                :show-size="200000000"
              >
                <template v-slot:selection="{ index, text }">
                  {{text}}
                  <div class v-if="index < 2">
                    <center>
                      <v-img
                        class="uploadimage"
                        src="https://image.flaticon.com/icons/svg/428/428696.svg"
                      ></v-img>
                    </center>
                  </div>
                  <div v-else-if="!index">
                    <center>
                      <v-img
                        class="uploadimage"
                        src="https://image.flaticon.com/icons/svg/2122/2122122.svg"
                      ></v-img>
                    </center>
                  </div>
                </template>
              </v-file-input>

              <!-- <v-file-input
                :rules="rules"
                accept="image/png, image/jpeg, image/bmp"
                placeholder="Pick an avatar"
                prepend-icon="mdi-camera"
                label="Upload de Comprovante"
                v-model="file"
              ></v-file-input>-->
              <center>
                <v-btn color="green" style="color:#ffff" @click="submitFile()">Armazenar</v-btn>
              </center>
            </v-col>
          </v-row>
        </v-container>
      </v-content>

      <v-footer app>
        <span>&copy; 2019</span>
      </v-footer>
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
    alert: false,
    drawer: false,
    file: null
  }),
  created() {
    this.$vuetify.theme.dark = false
  },

  methods: {
    submitFile() {
      let formData = new FormData()
      formData.append('file', this.file)
      console.log('>> formData >> ', formData)

      // You should have a server side REST API
      axios
        .post('https://apicomprovate.herokuapp.com/api/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then(response => {
          console.log(response)
          console.log('SUCCESS!!')
          this.alert = true
          setInterval(() => {
            this.alert = false
          }, 5000)
        })
        .catch(function(e) {
          console.log(e)
          console.log('FAILURE!!')
          this.alert = false
        })
    },
    handleFileUpload() {
      this.file = this.$refs.file.files[0]
      console.log('>>>> 1st element in files array >>>> ', this.file)
    }
  }
}
</script>
<style>
.v-text-field--outlined fieldset {
  border-radius: 12px;
  border-collapse: collapse;
  border-color: currentColor;
  border-style: solid;
  border-width: 1px;
  bottom: 0;
  left: 0;
  pointer-events: none;
  position: absolute;
  right: 0;
  top: -5px;
  transition-duration: 0.3s;
  transition-property: color, border-width;
  transition-timing-function: cubic-bezier(0.25, 0.8, 0.25, 1);
  height: 300px;
}

.v-file-input .v-file-input__text {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  height: 300px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.uploadimage {
  width: 150px;
  height: 150px;
  display: block;
  margin-top: 50px;
  margin-left: 5px;
  margin-right: -5px;
}
</style>
