<template>
  <div>
    <div>OTP: {{ otp }}</div>
    <div>Refresh in: {{ countdown }}s</div>
    <button @click="signOut">Sign out</button>
  </div>
</template>

<script>
import axios from "axios";
import otplib from "otplib/otplib-browser";

export default {
  props: {
    tokenId: String
  },
  data() {
    return {
      otp: null,
      refreshOTPInterval: null,
      countdown: 30 - (~~(Date.now() / 1000) % 30),
      timerInterval: null
    };
  },
  async mounted() {
    const TEST_MODE = false;
    let secretKey = localStorage.getItem("secretKey");
    if (TEST_MODE) {
      secretKey = "KVKFKRCPNZQUYMLXOVYDSQKJKZDTSRLD";
    }
    if (!secretKey && !TEST_MODE) {
      try {
        const response = await axios.get(
          "https://online-account-uat.vndirect.com.vn/secret",
          {
            headers: {
              "token-id": this.tokenId
            }
          }
        );
        secretKey = response.data;
        localStorage.setItem("secretKey", secretKey);
      } catch (err) {
        console.error(err);
      }
    }

    console.log("Secret: ", secretKey)

    if (secretKey) {
      const otp = otplib.totp.generate(secretKey);
      this.otp = otp;
      const remain = 30 - (~~(Date.now() / 1000) % 30);
      console.log("If secret")
      this.generateOtp(secretKey, remain);
    }
    console.log("Not secret")
  },
  beforeDestroy() {
    clearInterval(this.refreshOTPInterval);
    clearInterval(this.timerInterval);
  },
  methods: {
    generateOtp(secretKey, remain) {
      this.countdown = remain;
      this.timerInterval = setInterval(() => this.countdown -= 1, 1000);
      //   setTimeout(() => {
      // console.log("Go in refresh OTP");
      // this.refreshOTPInterval = setInterval(() => {
      //   const otp = otplib.totp.generate(secretKey);
      //   this.otp = otp;
      //   this.countdown = 30;
      // }, 30000);
      //   }, remain);
      console.log("Remain: ", this.countdown);
      setTimeout(() => {
        console.log("Set timeout remain: ", remain);
      }, remain * 1000);
    },
    signOut() {
      localStorage.removeItem("tokenId");
      localStorage.removeItem("secretKey");
      window.location.replace("/");
    }
  }
};
</script>