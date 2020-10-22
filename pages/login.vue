<template>
  <div class="container">
    <div class="text-center">
      <h1>Login</h1>
      <br />
    </div>
    <v-card class="card">
      <v-card-text>
        <h2>Sign in to start your session</h2>
        <v-form action="#.html" method="post">
          <v-text-field
            type="email"
            class="form-control"
            placeholder="Email"
          ></v-text-field>
          <v-text-field
            type="password"
            class="form-control"
            placeholder="Password"
          ></v-text-field>
          <div class="row">
            <div class="col">
              <v-checkbox
                id="remember"
                :label="`Remember Me`"
                type="checkbox"
              ></v-checkbox>
            </div>
            <div class="col-">
              <v-btn color="#F4D03F" class="mr-4" @click="login">
                Sign In
              </v-btn>
              <v-btn color="#F4D03F" class="mr-4" @click="loginGoogle">
                Sign In With Google
              </v-btn>
            </div>
          </div>
        </v-form>
        <hr />
        <br />
        <p class="mb-1">
          <a href="/login">I forgot my password</a>
        </p>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import firebase from 'firebase/app'

export default {
  methods: {
    loginGoogle() {
      const provider = new firebase.auth.GoogleAuthProvider()
      provider.addScope('https://www.googleapis.com/auth/contacts.readonly')
      firebase
        .auth()
        .signInWithPopup(provider)
        .then((result) => {
          // This gives you a Google Access Token. You can use it to access the Google API.
          const token = result.credential.accessToken
          // The signed-in user info.
          const user = result.user

          // ...
          console.log('token : ' + token)
          console.log('user : ' + user)
          this.$router.replace('/')
        })
        .catch((error) => {
          // Handle Errors here.
          const errorCode = error.code
          const errorMessage = error.message
          // The email of the user's account used.
          const email = error.email
          // The firebase.auth.AuthCredential type that was used.
          const credential = error.credential
          // ...
          console.log('errorCode= ' + errorCode)
          console.log('errorMessage= ' + errorMessage)
          console.log('email= ' + email)
          console.log('credential' + credential)
        })
    },
  },
}
</script>

<style></style>
