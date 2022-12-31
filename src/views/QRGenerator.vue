<template>
  <v-container>
    <v-responsive class="d-flex align-center text-center">
      <div class="d-flex justify-center">
        <p class="text-h3 font-weight-bold">QR&nbsp;Coder</p>
        <p class="text-caption">V1.0.0</p>
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
        @update:model-value="generate"
      ></v-textarea>

      <div class="py-2"></div>

      <v-btn
        @click="generate"
      >
        Generate
      </v-btn>

      <div class="py-2"></div>

      <div class="d-flex justify-center" >
        <div class="qrcode">
          <v-img
            class="rounded-xxl"
            aspect-ratio="1"
            :src="image"
          ></v-img>
        </div>
      </div>

      <div class="py-2"></div>
    </v-responsive>
  </v-container>
</template>

<script setup>
import {ref} from 'vue'
import { useAppStore } from '@/store/app.js'
import QRCode from 'qrcode'
import defaultQr from '@/assets/default_qr.png'

const store = useAppStore()

const data = ref('')
const image = ref(defaultQr)


const generate = async () => {
  if (!data.value) {
    store.showSnackbar('Missing data!')
    return
  }


  try {
    const qrcode = await QRCode.toDataURL(data.value)
    image.value = qrcode
  } catch (err) {
    console.error(err)
  }

}
</script>

<style scoped>
.qrcode {
  width: 80%;
  height: 80%;
}
.rounded-xxl {
  border-radius: 80px;
}
</style>