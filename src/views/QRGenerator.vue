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

      <v-btn-toggle
        v-model="type"
        @click="initValue"
      >
        <v-btn icon="mdi-text" color="primary"></v-btn>
        <v-btn icon="mdi-wifi" color="primary"></v-btn>
        <v-btn icon="mdi-account" color="primary"></v-btn>
        <v-btn icon="mdi-phone" color="primary"></v-btn>
        <v-btn icon="mdi-email" color="primary"></v-btn>
        <v-btn icon="mdi-calendar" color="primary"></v-btn>
      </v-btn-toggle>

      <div class="py-2"></div>
      
      <v-textarea 
        v-if="type==0"
        label="Data"
        v-model="data"
        placeholder="Content of the QR Code"
        hint="Type or paste the data of the QR Code"
        @update:model-value="generate"
      ></v-textarea>

      <v-container
        v-if="type==1"
      >
        <v-combobox
          label="Authentication Type"
          :items="['WEP', 'WPA', 'nopass']"
          v-model="auth"
          @click="generate"
        ></v-combobox>
        <v-text-field
          label="SSID"
          v-model="ssid"
          @update:model-value="generate"
        ></v-text-field>
        <v-text-field
          label="Password"
          :type="showPwd ? 'text' : 'password'"
          :append-icon="showPwd ? 'mdi-eye' : 'mdi-eye-off'"
          @click:append="showPwd = !showPwd"
          v-model="sidPassword"
          @update:model-value="generate"
        ></v-text-field>
        <v-switch 
          label="Hidden" 
          inset 
          color="primary"
          @update:model-value="nextTick(generate)"
          v-model="hidden"
        ></v-switch>
      </v-container>

      <v-container
        v-if="type==2"
      >
      </v-container>

      <v-container
        v-if="type==3"
      >
        <v-text-field
          label="Phonenumber"
          v-model="phonenumber"
          @update:model-value="generate"
        ></v-text-field>
      </v-container>

      <v-container
        v-if="type==4"
      >
        <v-text-field
          label="Email"
          v-model="email"
          @update:model-value="generate"
        ></v-text-field>
        <v-text-field
          label="Subject"
          v-model="subject"
          @update:model-value="generate"
        ></v-text-field>
        <v-textarea
          label="Message"
          v-model="message"
          @update:model-value="generate"
        ></v-textarea>
      </v-container>

      <v-container
        v-if="type==5"
      >
      </v-container>

      <v-container
        v-if="type==6"
      >
      </v-container>

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
            class="rounded-xl"
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
import {ref, nextTick} from 'vue'
// import { useAppStore } from '@/store/app.js'
import QRCode from 'qrcode'
import defaultQr from '@/assets/default_qr.png'

// const store = useAppStore()

const data = ref('')
const image = ref(defaultQr)

const type = ref(0)
const showPwd = ref(false)

const auth = ref('WPA');
const hidden = ref(false);
const ssid = ref();
const sidPassword = ref();

const phonenumber = ref();

const email = ref();
const subject = ref();
const message = ref();

const generate = async () => {
  switch (type.value) {
    case 0:
      break
    case 1:
      data.value = `WIFI:T:${auth.value};S:${ssid.value?ssid.value:''};P:${sidPassword.value?sidPassword.value:''};${hidden.value?'H:true':''};`
      console.log(data.value)
      break  
    case 2:
      //vcard
      data.value = `BEGIN:VCARD
VERSION:3.0
N:<lastname>:<firstname>
FN:<firstname> <lastname>
TITLE:<title>
ORG:<org>
URL:<url>
EMAIL;TYPE=INTERNET:<email>
TEL;TYPE=voice,work,pref:<phone work>
TEL;TYPE=voice,home,pref:<phone private>
TEL;TYPE=voice,cell,pref:<phone mobile>
TEL;TYPE=fax,work,pref:<fax work>
TEL;TYPE=fax,home,pref:<fax home>
ADR:;;<street>;<city>;<state>;<zip>;<country>
END:VCARD`
      break
    case 3:
      data.value = `tel:${phonenumber.value?phonenumber.value:''}`
      break
    case 4:
      data.value = `MATMSG:TO:${email.value?email.value:''};SUB:${subject.value?subject.value:''};BODY:${message.value?message.value:''};;`
      break
    case 5:
      //calendar
      data.value = `BEGIN:VEVENT
SUMMARY:<event name>
LOCATION:<location>
DTSTART:YYYYMMDDTHHMMSS
DTEND:YYYYMMDDTHHMMSS
END:VEVENT`
      break
  }

  if (!data.value) {
    image.value = defaultQr
    return
  }

  try {
    const qrcode = await QRCode.toDataURL(data.value)
    image.value = qrcode
  } catch (err) {
    console.error(err)
  }

}

const initValue = async () => {
  if (type.value!=0) {
    data.value = ""
    generate()
  }
}
</script>

<style scoped>
.qrcode {
  width: 70%;
  height: 70%;
}
.rounded-xxl {
  border-radius: 80px;
}
</style>