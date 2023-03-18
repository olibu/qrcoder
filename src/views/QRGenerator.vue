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
      <v-form
        ref="formGeneral"
      >
      
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
            :rules="ruleNotEmpty"
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
          <v-text-field
            label="First Name"
            v-model="firstname"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Name"
            v-model="surname"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Title"
            v-model="title"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Company"
            v-model="company"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Phone Home"
            v-model="phonenumber"
            :rules="rulePhone"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Phone Work"
            v-model="phoneWork"
            :rules="rulePhone"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Mobile"
            v-model="phoneMobile"
            :rules="rulePhone"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Email"
            v-model="email"
            :rules="ruleEmail"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Homepage"
            v-model="homepage"
            :rules="ruleUrl"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Street"
            v-model="street"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Zip"
            v-model="zip"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="City"
            v-model="city"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="State"
            v-model="region"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Country"
            v-model="country"
            @update:model-value="generate"
          ></v-text-field>
        </v-container>

        <v-container
          v-if="type==3"
        >
          <v-text-field
            label="Phonenumber"
            v-model="phonenumber"
            :rules="[ruleNotEmpty, rulePhone].flat()"
            @update:model-value="generate"
          ></v-text-field>
        </v-container>

        <v-container
          v-if="type==4"
        >
          <v-text-field
            label="Email"
            v-model="email"
            :rules="[ruleEmail, ruleNotEmpty].flat()"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Subject"
            v-model="subject"
            :rules="ruleNotEmpty"
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
          <v-text-field
            label="Eventname"
            v-model="eventname"
            :rules="ruleNotEmpty"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="Location"
            v-model="location"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="From"
            placeholder="YYYYMMDDTHHMMSS (e.g. 20231224T190000)"
            hint="Skip time and to in case of all day event"
            :rules="[ruleNotEmpty, ruleDate].flat()"
            v-model="fromDate"
            @update:model-value="generate"
          ></v-text-field>
          <v-text-field
            label="To"
            placeholder="YYYYMMDDTHHMMSS (e.g. 20231224T230000)"
            hint="Skip in case of all day event"
            v-model="toDate"
            :rules="ruleDate"
            @update:model-value="generate"
          ></v-text-field>
        </v-container>

      </v-form>

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

const eventname = ref();
const location = ref();
const fromDate = ref();
const toDate = ref();

const firstname = ref();
const surname = ref();
const title = ref();
const company = ref();
const homepage = ref();
const phoneWork = ref();
const phoneMobile = ref();
const street = ref();
const region = ref();
const city = ref();
const zip = ref();
const country = ref();

const formGeneral = ref()

const ruleEmail = [
  value => {
    if (!value) {
      return true
    }
    const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
    return pattern.test(value) || 'Invalid e-mail.'
  },
]
const rulePhone = [
  value => {
    if (!value) {
      return true
    }
    const pattern = /(^(\+)|(0))([0-9./\- ]+)$/
    return pattern.test(value) || 'Invalid phone number.'
  },
]
const ruleUrl = [
  value => {
    if (!value) {
      return true
    }
    const pattern = /(^http:\/\/)|(^https:\/\/)/
    return pattern.test(value) || 'Invalid URL.'
  },
]
const ruleDate = [
  value => {
    if (!value) {
      return true
    }
    const pattern = /[0-9T]+/
    return pattern.test(value) || 'Invalid date format use <YYYYMMDD>T[HHMMSS]'
  },
]
const ruleNotEmpty = [
  value => !!value || 'Required.',
]

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
N:${surname.value?surname.value:''};${firstname.value?firstname.value:''}
FN:${firstname.value?firstname.value+' ':''}${surname.value?surname.value:''}
TITLE:${title.value?title.value:''}
ORG:${company.value?company.value:''}
URL:${homepage.value?homepage.value:''}
EMAIL:${email.value?email.value:''}
TEL;TYPE=voice,home,pref:${phonenumber.value?phonenumber.value:''}
TEL;TYPE=voice,work,pref:${phoneWork.value?phoneWork.value:''}
TEL;TYPE=voice,cell,pref:${phoneMobile.value?phoneMobile.value:''}
ADR:;;${street.value?street.value:''};${city.value?city.value:''};${region.value?region.value:''};${zip.value?zip.value:''};${country.value?country.value:''}
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
SUMMARY:${eventname.value?eventname.value:''}
LOCATION:${location.value?location.value:''}
DTSTART:${fromDate.value?fromDate.value:''}
DTEND:${toDate.value?toDate.value:''}
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
  // show the original logo
  if (type.value!=0) {
    data.value = ""
    generate()
  }

  // validate the active form to show mandatory fields
  formGeneral.value.validate()
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