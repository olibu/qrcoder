<template>
  <v-container>
    <v-responsive class="d-flex align-center text-center">
      <div class="d-flex justify-center">
        <p class="text-h3 font-weight-bold">QR&nbsp;Coder</p>
        <p class="text-caption">V0.0.1</p>
      </div>

      <div class="py-2"></div>
      
      <div class="text-body-2 font-weight-light mb-n1">
        Welcome to the QR Coder, an offline generator for QR Codes.
      </div>

      <div class="py-2"></div>

      <v-textarea 
        label="Data"
        v-model="data"
        placeholder="Content of the QR Code"
        hint="Type or paste the data of the QR Code"
      ></v-textarea>
      <v-btn
        @click="generate"
      >
        Generate
      </v-btn>

      <div class="py-2"></div>

      <canvas id="canvas" :style="{ backgroundImage: `url(${imagePath})` }"></canvas>

      <v-img
      v-model="image"
      aspect-ratio="1"
      width="100"
      src="https://cdn.vuetifyjs.com/images/parallax/material.jpg"
      cover
    ></v-img>

      <div class="py-2"></div>
    </v-responsive>
  </v-container>
</template>

<script setup>
import {ref} from 'vue'
import { useAppStore } from '@/store/app.js'
import QRCode from 'qrcode'

const store = useAppStore()

const data = ref('')
const image = ref()


const generate = async () => {
  if (!data.value) {
    store.showSnackbar('Missing data!')
    return
  }


  try {
    const qrcode = await QRCode.toDataURL(data.value)
    const canvas = document.getElementById('canvas')
    image.value = qrcode
  } catch (err) {
    console.error(err)
  }

}

const loadImage = (base64img) => {
  const canvas = document.getElementById('canvas')
  const ctx = canvas.getContext("2d")
  const reader = new FileReader
  reader.onload = function (event) {
    const img = new Image()
    img.src = reader.result
    img.onload = function () {
      ctx.drawImage(this, 0, 0, canvas.width, canvas.height)
    }
  }
  reader.readAsDataURL(base64img);
}
</script>
