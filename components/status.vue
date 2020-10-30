<template>
  <v-card>
    <v-data-table
      :headers="headers"
      :items="textList"
      :search="search"
    ></v-data-table>
  </v-card>
</template>

<script>
import { db } from '~/plugins/firebaseConfig.js'
export default {
  data() {
    return {
      search: '',
      headers: [
        { text: 'หมายเลขติดตามพัสดุ', value: 'track', align: 'start' },
        { text: 'ชื่อ-สกุล', value: 're_name' },
        { text: 'ติดต่อ', value: 're_phone' },
        { text: 'วันที่ทำรายการ', value: 'date' },
        { text: 'สถานะ', value: 'status' },
      ],
      textList: [],
    }
  },
  created() {
    this.getData()
  },
  methods: {
    getData() {
      db.collection('Update Status')
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
