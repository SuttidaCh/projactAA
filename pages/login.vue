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
            </div>
          </div>
        </v-form>
        <hr />
        <br />
        <p class="mb-1">
          <a href="/login">I forgot my password</a>
        </p>
        <p class="mb-0">
          <a href="/login" class="text-center">Register a new membership</a>
        </p>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {}
</script>

<style></style>
