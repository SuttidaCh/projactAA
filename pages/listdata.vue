<template>
  <v-card>
    <v-card-title>
      ข้อมูลลูกค้า
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>

    <v-data-table :headers="headers" :items="textList" :search="search">
      ></v-data-table
    >
  </v-card>
</template>

<script>
import firebase from 'firebase/app'
import { db } from '~/plugins/firebaseConfig.js'
export default {
  data() {
    return {
      search: '',
      headers: [
        { text: 'หมายเลขติดตามพัสดุ', value: 'tracking' },
        { text: 'ชื่อ-สกุล', value: 'name' },
        { text: 'วันที่รับ', value: 'date' },
        { text: 'ที่อยู่', value: 'address' },
        { text: 'อำเภอ', value: 'district' },
        { text: 'จังหวัด', value: 'province' },
        { text: 'รหัสไปรษณีย์', value: 'postcode' },
        { text: 'สถานะ', value: 'status' },
        { text: 'รวมเป็นเงิน', value: 'total' },
      ],
      textList: [],
    }
  },
  created() {
    this.getData()
  },
  methods: {
    beforeCreate() {
      if (!firebase.auth().currentUser) {
        console.log('No Login')
      } else {
        console.log('Login ok')
      }
    },
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
