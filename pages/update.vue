<template>
  <v-container>
    <v-form ref="form" @submit.prevent="addData">
      <h1>Update Status</h1>
      <br />
      <div class="row md-5">
        <v-text-field
          v-model="address"
          :rules="addressRules"
          label="Tracking Number/Scan QrCode"
          class="ma-3"
          solo
          required
        ></v-text-field>
      </div>
      <div class="text-center">
        <v-btn color="#F4D03F" class="mr-4" @click="addData">Update</v-btn>
      </div>
    </v-form>
  </v-container>
</template>

<script>
export default {}
</script>

<style></style>
