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
      {{ item.postcode }}
    </v-card>
  </v-card>
</template>

<script>
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
  methods: {
    getData() {
      db.collection('MyText')
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
