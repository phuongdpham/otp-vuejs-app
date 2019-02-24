<template>
    <div>
        <div>OTP: {{ otp }}</div>
        <div>Refresh in: {{ countdown }}s</div>
        <button @click="signOut">Sign out</button>
    </div>
</template>

<script>
import axios from 'axios'
import otplib from 'otplib/otplib-browser'

export default {
  props: {
    tokenId: String
  },
  data() {
      return {
          otp: null,
          refreshOTPInterval: null,
          countdown: 30,
          timerInterval: null,
      }
  },
  async mounted() {
      const TEST_MODE = true
      let secretKey = localStorage.getItem('secretKey')
      if (TEST_MODE) {
          secretKey = 'KVKFKRCPNZQUYMLXOVYDSQKJKZDTSRLD'
      }
      if (!secretKey && !TEST_MODE) {
        try {
            const response = await axios.get('https://online-account-uat.vndirect.com.vn/secret', {
                headers: {
                    'token-id': this.tokenId,
                },
            })
            secretKey = response.data
            localStorage.setItem('secretKey', secretKey)
        } catch (err) {
            console.error(err)
        }
      }

      if (secretKey) {
            const otp = otplib.authenticator.generate(secretKey)
            this.otp = otp
            this.generateOtp(secretKey)
      }
  },
  beforeDestroy() {
      clearInterval(this.refreshOTPInterval)
      clearInterval(this.timerInterval)
  },
  methods: {
      generateOtp(secretKey) {
            this.timerInterval = setInterval(() => {
                this.countdown -= 1
            }, 1000)
            this.refreshOTPInterval = setInterval(() => {
                const otp = otplib.authenticator.generate(secretKey)
                this.otp = otp
                this.countdown = 30
            }, 30000)
      },
      signOut() {
          localStorage.removeItem('tokenId')
          localStorage.removeItem('secretKey')
          window.location.replace('/')
      }
  }
}
</script>