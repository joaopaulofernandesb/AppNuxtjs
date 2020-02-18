<template>
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
        <v-list-item link>
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
      <v-toolbar-title>Application</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container>
        <v-row align="center" justify="center">
          <v-col>
            <v-file-input
              :rules="rules"
              accept="image/png, image/jpeg, image/bmp"
              placeholder="Pick an avatar"
              prepend-icon="mdi-camera"
              label="Avatar"
              v-model="file"
            ></v-file-input>
            <v-btn small @click="submitFile()">Normal</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer app>
      <span>&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
import axios from 'axios'
export default {
  props: {
    source: String
  },
  data: () => ({
    drawer: false,
    file: '',
    rules: [
      value =>
        !value ||
        value.size < 2000000 ||
        'Avatar size should be less than 2 MB!'
    ]
  }),
  created() {
    this.$vuetify.theme.dark = true
  },
  methods: {
    submitFile() {
      let formData = new FormData()
      formData.append('file', this.file)
      console.log('>> formData >> ', formData)

      // You should have a server side REST API
      axios
        .post('https://ef36bedd.ngrok.io/api/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then(function() {
          console.log('SUCCESS!!')
        })
        .catch(function() {
          console.log('FAILURE!!')
        })
    },
    handleFileUpload() {
      this.file = this.$refs.file.files[0]
      console.log('>>>> 1st element in files array >>>> ', this.file)
    }
  }
}
</script>
