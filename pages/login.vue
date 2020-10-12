<template>
  <div class="login-box">
    <div class="text-center">
      <h1>Login</h1>
      <br />
    </div>
    <!-- /.login-logo -->
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
            <div class="col-10">
              <v-checkbox
                id="remember"
                :label="`Remember Me`"
                type="checkbox"
              ></v-checkbox>
            </div>
            <div class="col-2">
              <v-btn color="#F4D03F" class="mr-4" @click="login">
                Sign In
              </v-btn>
            </div>
          </div>
        </v-form>
        <div class="social-auth-links text-center mb-3">
          <p>- OR -</p>
          <a href="#" class="btn btn-block btn-primary">
            <i class="fab fa-facebook mr-2"></i> Sign in using Facebook
          </a>
          <a href="#" class="btn btn-block btn-danger">
            <i class="fab fa-google-plus mr-2"></i> Sign in using Google+
          </a>
        </div>
        <!-- /.social-auth-links -->
        <hr />
        <br />
        <p class="mb-1">
          <a href="forgot-password.html">I forgot my password</a>
        </p>
        <p class="mb-0">
          <a href="register.html" class="text-center"
            >Register a new membership</a
          >
        </p>
      </v-card-text>
      <!-- /.login-card-body -->
    </v-card>
  </div>
</template>

<script>
export default {}
</script>

<style></style>
