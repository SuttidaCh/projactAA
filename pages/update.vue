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
    <br />
    <v-data-table
      :headers="dessertHeaders"
      :items="textList"
      :single-expand="singleExpand"
      :expanded.sync="expanded"
      item-key="name"
      show-expand
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Expandable Table</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-switch
            v-model="singleExpand"
            label="Single expand"
            class="mt-2"
          ></v-switch>
        </v-toolbar>
      </template>
      <template v-slot:expanded-item="{ headers, item }">
        <td :colspan="headers.length">More info about {{ item.name }}</td>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
import { db } from '~/plugins/firebaseConfig.js'
export default {
  data() {
    return {
      expanded: [],
      singleExpand: false,
      dessertHeaders: [
        { text: 'หมายเลขติดตามพัสดุ', value: 'tracking' },
        { text: 'ชื่อ-สกุล', value: 'name' },
        { text: 'วันที่รับ', value: 'date' },
        { text: 'ที่อยู่', value: 'address' },
        { text: 'อำเภอ', value: 'district' },
        { text: 'จังหวัด', value: 'district' },
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
