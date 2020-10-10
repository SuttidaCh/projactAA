<template>
  <div class="container">
    <h1>Contact Us</h1>
    <br />
    <v-form id="contactForm" ref="form" name="sentMessage">
      <div class="row align-items-stretch mb-5">
        <div class="col-md-6">
          <div class="form-group">
            <v-text-field
              id="name"
              class="form-control"
              type="text"
              placeholder="ชื่อ-สกุล"
              required
              data-validation-required-message="Please enter your name."
            ></v-text-field>
            <p class="help-block text-danger"></p>
          </div>
          <div class="form-group">
            <v-text-field
              id="email"
              class="form-control"
              type="email"
              placeholder="Email"
              required
              data-validation-required-message="Please enter your email address."
            ></v-text-field>
            <p class="help-block text-danger"></p>
          </div>
          <div class="form-group mb-md-0">
            <v-text-field
              id="phone"
              class="form-control"
              type="tel"
              placeholder="เบอร์โทร"
              required
              data-validation-required-message="Please enter your phone number."
            ></v-text-field>
            <p class="help-block text-danger"></p>
          </div>
        </div>
        <div class="col-md-6">
          <div class="form-group form-group-textarea mb-md-0">
            <v-text-field
              id="message"
              class="form-control"
              placeholder="ข้อความติดต่อ"
              required
              data-validation-required-message="Please enter a message."
            ></v-text-field>
            <p class="help-block text-danger"></p>
          </div>
        </div>
      </div>
      <div class="text-center">
        <v-btn
          id="sendMessageButton"
          color="#F4D03F"
          class="btn btn-primary btn-xl text-uppercase"
          type="submit"
        >
          Send Message
        </v-btn>
      </div>
    </v-form>
  </div>
</template>

<script>
export default {}
</script>

<style></style>
