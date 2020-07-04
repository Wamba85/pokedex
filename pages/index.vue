<template>
  <v-container class="text-center pt-10">
    <v-row class="rounded-sm pokedex">
      <v-col cols="6" class="pa-10">
        <v-row justify="center">
          <v-img max-width="80%" src="pokedex.png" />
        </v-row>
        <v-row class="monitor grey lighten-2 black--text pa-8 mt-8">
          <v-col cols="12" class="white">
            <v-img contain max-height="100%" :src="image" />
          </v-col>
        </v-row>
        <v-row justify="space-around" class="mt-8">
          <input ref="fileInput" style="display: none;" type="file" @change="onFileChanged" />
          <v-btn
            elevation="6"
            class="rounded-circle py-15 choose light-blue--text text--darken-4"
            @click="$refs.fileInput.click()"
          >
            Choose image
          </v-btn>
          <v-btn elevation="6" class="rounded-circle py-12 predict green--text text--darken-4" @click="onUpload">
            Predict!
          </v-btn>
        </v-row>
      </v-col>
      <v-col cols="1" class="middle middle-gradient-light">
        <v-row class="fill-height flex-column flex-nowrap">
          <v-col class="middle-gradient"></v-col>
          <v-col class="grow"></v-col>
          <v-col class="middle-gradient"></v-col>
        </v-row>
      </v-col>
      <v-col cols="5" align-self="center" class="pa-10">
        <v-row align="center" justify="center" class="blue lighten-5 black--text min-height">{{ prediction }}</v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      selectedFile: null,
      image: 'pokemon-placeholder.png',
      prediction: 'antani',
    }
  },
  methods: {
    onFileChanged(event) {
      this.selectedFile = event.target.files[0]
      const reader = new FileReader()
      reader.onload = (e) => {
        return (this.image = e.target.result)
      }
      reader.readAsDataURL(event.target.files[0])
    },
    onUpload() {
      const formData = new FormData()
      formData.append('file', this.selectedFile, this.selectedFile.name)
      this.$axios
        .post('http://localhost:5000/api/v1.0/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then((res) => {
          this.prediction = res.data
        })
    },
  },
}
</script>

<style scoped>
.pokedex {
  height: 80vh;
  width: 100%;
  background-color: #d60a2c;
  border: #600606 solid 5px;
  border-radius: 5% !important;
}
.pokedex-color {
  background-color: #d60a2c;
}
.pokedex-color-dark {
  background-color: #bf0c0c;
}
.monitor {
  height: 40vh;
}
.grow {
  flex-grow: 10 !important;
}
.middle {
  border: #600606 solid 1px;
}
.middle-gradient {
  background: linear-gradient(to right, #bf0c0c, #d60a2c, #bf0c0c);
}
.middle-gradient-light {
  background: linear-gradient(to right, #d60a2c, #ca455b, #d60a2c);
}
.min-height {
  min-height: 20vh;
}
.choose {
  background-color: #b3e5fc !important;
  border: #01579b solid 3px;
}
.predict {
  background-color: #00e676 !important;
  border: #1b5e20 solid 3px;
}
</style>
