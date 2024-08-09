<template>
  <div class="signup-page">
    <div class="signup-form">
      <h1 class="form-title">Sign Up Form</h1>
      <v-form @submit.prevent>
        <div v-if="registerSteps.phoneStep">
          <div class="mobile-section">
            <v-select class="country-field" v-model="countryCode" label="Country"
              :items="['+98', '+1', '+49']"></v-select>
            <v-text-field class="mobile-field" v-model="phoneNumber" label="Mobile" type="number"></v-text-field>
          </div>
          <v-btn :disabled="countryCode === '' || phoneNumber.length !== 10"
            class="mt-2" color="#5E578A" type="submit" block @click="registerAccount">Submit</v-btn>
        </div>
        <div v-else-if="registerSteps.otpStep">
          <div>
            <h3>code sended to {{ countryCode }} {{ phoneNumber }}</h3>
          </div>
          <v-otp-input :length="4" placeholder="-" v-model="otpCode"></v-otp-input>
          <v-btn class="mt-2" color="#5E578A" type="submit" block
            @click="postNumber">Submit</v-btn>
        </div>
        <div v-if="loading">loading</div>
      </v-form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'


const registerSteps = ref({
  phoneStep: true,
  otpStep: false
})

const phoneNumber = ref('')
const countryCode = ref('+98')
const otpCode = ref('')

const loading = ref(false)

const registerAccount = async () => {
  loading.value = true;
  const response = await axios.post('https://dummyjson.com/auth/login' , 
  {
  username: 'emilys',
  password: 'emilyspass',
  expiresInMins: 30, // optional, defaults to 60
}
  );
  console.log(response)
  loading.value = false
  registerSteps.value.phoneStep = false
  registerSteps.value.otpStep = true
}



const postNumber = async () => {
  console.log('test')

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.signup-page {
  width: 100%;
  height: 80vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.signup-form {
  width: clamp(300px, 100%, 400px);

}

.form-title {
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 1.5rem;
  color: #5E578A;
}

.mobile-section {
  width: 100%;
  /* background-color: yellow; */
  display: flex;
  align-items: center;
}

.country-field {
  width: 30px;
  margin-right: 8px;
  /* background-color: red; */

}

.mobile-field {
  /* background-color: green; */
  width: 150px;
}
</style>