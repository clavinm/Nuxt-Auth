<template>
    <v-container class="px-0">
      <div class="facebook-btn">
        <v-btn
          color="#3b5998"
          dark
          medium
          class="text-none"
          @click="logInWithFacebook"
        >
          <v-icon class="mr-2">mdi-facebook</v-icon>
          <span>Sign in with Facebook</span>
        </v-btn>
      </div>
    </v-container>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'


  const logInWithFacebook = () => {
    window.FB.login(function (response) {
      if (response.authResponse) {
        const accessToken = response.authResponse.accessToken;

        // Store the access token in localStorage
        localStorage.setItem('userToken', accessToken);
        emit('loggedIn')
      } else {
        console.log('User cancelled login or did not fully authorize.')
      }
    })
    return false
  }
  
  const initFacebook = () => {
    window.fbAsyncInit = function () {
      window.FB.init({
        appId: '270520542820697', //App ID, You will need to change this
        cookie: true,
        xfbml: true,
        version: 'v11.0'
      })
    }
  }
  
  const loadFacebookSDK = (d, s, id) => {
    const fjs = d.getElementsByTagName(s)[0]
    if (d.getElementById(id)) {
      return
    }
    const js = d.createElement(s)
    js.id = id
    js.src = 'https://connect.facebook.net/en_US/sdk.js'
    fjs.parentNode.insertBefore(js, fjs)
  }
  
  
  onMounted(() => {
    loadFacebookSDK(document, 'script', 'facebook-jssdk')
    initFacebook();
    
  })
  </script>
  