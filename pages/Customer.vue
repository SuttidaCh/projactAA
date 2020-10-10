<template>
  <v-container>
    <v-form ref="form" @submit.prevent="addData">
      <h1>ข้อมูลลูกค้า</h1>
      <br />
      <div class="row md-5">
        <v-text-field
          v-model="name"
          :rules="nameRules"
          label="ชื่อ-สกุล"
          class="ma-3"
          required
        ></v-text-field>
        <v-text-field
          v-model="phone"
          :rules="phoneRules"
          label="เบอร์โทร"
          class="ma-3"
          required
        ></v-text-field>
      </div>
      <div class="row md-5">
        <v-text-field
          v-model="address"
          :rules="addressRules"
          label="ที่อยู่"
          class="ma-3"
          required
        ></v-text-field>
      </div>
      <div class="row md-5">
        <v-text-field
          v-model="postcode"
          :rules="postcodeRules"
          label="รหัสไปรษณีย์"
          class="ma-3"
          required
        ></v-text-field>
      </div>
      <br />
      <div>ปลายทาง</div>
      <v-radio-group v-model="area" row>
        <v-radio :label="`กรุงเทพฯ`" :value="t1" required></v-radio>
        <v-radio :label="`ต่างจังหวัด`" :value="t2" required></v-radio>
      </v-radio-group>
      <br />
      <div>ขนาดกล่อง</div>
      <v-radio-group v-model="box" row>
        <v-radio :label="`S`" :value="S" required></v-radio>
        <v-radio :label="`M`" :value="M" required></v-radio>
        <v-radio :label="`L`" :value="L" required></v-radio>
        <v-radio :label="`XL`" :value="XL" required></v-radio>
      </v-radio-group>
      <br />
      <v-btn color="#7B7D7D" class="mr-4" @click="addData">Submit</v-btn>
      <v-btn color="#F4D03F" class="mr-4" @click="reset"> Reset Form </v-btn>
    </v-form>
    <CollectionText />
  </v-container>
</template>

<script>
import firebase from 'firebase/app'
import { db } from '~/plugins/firebaseConfig.js'
import CollectionText from '~/components/CollectionText.vue'
export default {
  components: {
    CollectionText,
  },
  data() {
    return {
      name: '',
      nameRules: [(v) => !!v || 'Name is required'],
      phone: '',
      phoneRules: [
        (v) => !!v || 'Phone is required',
        (v) => (v && v.length <= 10) || 'Phone must be less than 10 characters',
      ],
      address: '',
      addressRules: [(v) => !!v || 'Address is required'],
      postcode: '',
      postcodeRules: [
        (v) => !!v || 'Post code is required',
        (v) =>
          (v && v.length <= 5) || 'Post code must be less than 10 characters',
      ],
      area: null,
      box: null,
    }
  },

  methods: {
    getData() {
      db.collection('MyForm')
        .doc('formdata')
        .onSnapshot((doc) => {
          // console.log("Current data: ", doc.data());
          const firebaseData = doc.data()
          if (firebaseData) {
            this.name = firebaseData.name
            this.phone = firebaseData.phone
            this.address = firebaseData.address
            this.postcode = firebaseData.postcode
            this.area = firebaseData.area
            this.box = firebaseData.box
          }
        })
    },
    created() {
      this.getData()
    },
    addData() {
      // เก็บข้อมูล Form ใน collection MyForm ( มี 1 document แต่จะ update ข้อมูลเรื่อย ๆ )
      const data = {
        name: this.name,
        phone: this.phone,
        postcode: this.postcode,
        area: this.area,
        box: this.box,
        timestamp: firebase.firestore.FieldValue.serverTimestamp(),
      }
      db.collection('MyForm')
        .doc()
        .set(data)
        .then(function () {
          console.log('Document successfully written! -> MyForm')
        })
        .catch(function (error) {
          console.error('Error writing document: ', error)
        })

      // เก็บข้อมูล Input Text ใน collection MyText (มีหลาย document ข้อมูลจะเพิ่มขึ้นเรื่อย ๆ )แสดงผลออกมาที่ด้านนนอก
      const dataText = {
        name: this.name,
        phone: this.phone,
        address: this.address,
        postcode: this.postcode,
        timestamp: firebase.firestore.FieldValue.serverTimestamp(),
      }
      db.collection('MyText')
        .doc()
        .set(dataText)
        .then(function () {
          console.log('Document successfully written! -> MyText')
        })
    },
    reset() {
      this.$refs.form.reset()
    },
  },
}
</script>

<style></style>
