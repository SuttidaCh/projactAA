<template>
  <v-card class="mt-5">
    <v-card
      v-for="(item, key) in textList"
      :key="key"
      class="pa-2"
      outlined
      tile
    >
      {{ item.name }}
      {{ item.phone }}
      {{ item.address }}
      {{ item.exstates }}
    </v-card>
  </v-card>
</template>

<script>
import firebase from 'firebase/app'
import { db } from '~/plugins/firebaseConfig.js'

export default {
  data() {
    return {
      textList: [],
    }
  },
  created() {
    this.getData()
  },
  beforeCreate() {
    if (!firebase.auth().currentUser) {
      console.log('No Login')
      this.$router.replace('/login')
    } else {
      console.log('Login ok')
    }
  },
  methods: {
    getData() {
      db.collection('Customer')
        .orderBy('timestamp')
        .onSnapshot((querySnapshot) => {
          const data = []
          querySnapshot.forEach((doc) => {
            data.push(doc.data())
          })
          this.textList = data
        })
    },
  },
}
</script>

<style></style>
