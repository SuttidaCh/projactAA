<template>
  <v-container>
    <v-form @submit.prevent="addData">
      <h1>ข้อมูลลูกค้า</h1>
      <br />
      <div class="row md-5">
        <v-text-field
          v-model="txt"
          label="ชื่อ-สกุล"
          class="ma-3"
        ></v-text-field>
        <v-text-field
          v-model="txt2"
          label="เบอร์โทร"
          class="ma-3"
        ></v-text-field>
      </div>
      <div class="row md-5">
        <v-text-field
          v-model="txt3"
          label="ที่อยู่"
          class="ma-3"
        ></v-text-field>
      </div>
      <div class="row md-5">
        <v-text-field
          v-model="txt4"
          label="รหัสไปรษณีย์"
          class="ma-3"
        ></v-text-field>
      </div>
      <br />
      <div>ปลายทาง</div>
      <v-radio-group v-model="radioGroup" row>
        <v-radio :key="n" :label="`กรุงเทพฯ`" :value="n"></v-radio>
        <v-radio :key="n" :label="`ต่างจังหวัด`" :value="n"></v-radio>
      </v-radio-group>
      <br /><br />
      <div>ขนาดกล่อง</div>
      <v-radio-group v-model="radioGroup2" row>
        <v-radio :key="n" :label="`S`" :value="n"></v-radio>
        <v-radio :key="n" :label="`M`" :value="n"></v-radio>
        <v-radio :key="n" :label="`L`" :value="n"></v-radio>
        <v-radio :key="n" :label="`XL`" :value="n"></v-radio>
      </v-radio-group>
      <br />
      <v-btn color="success" class="mr-4" @click="addData">Submit</v-btn>
      <v-btn color="warning" class="mr-4" @click="reset"> Reset Form </v-btn>
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
      txt: '',
      txt2: '',
      txt3: '',
      txt4: '',
      radioGroup: null,
      radioGroup2: null,
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
            this.txt = firebaseData.txt
            this.txt2 = firebaseData.txt2
            this.txt3 = firebaseData.txt3
            this.txt4 = firebaseData.txt4
            this.radioGroup = firebaseData.radioGroup
            this.radioGroup2 = firebaseData.radioGroup2
          }
        })
    },
    created() {
      this.getData()
    },
    addData() {
      // เก็บข้อมูล Form ใน collection MyForm ( มี 1 document แต่จะ update ข้อมูลเรื่อย ๆ )
      const data = {
        txt: this.txt,
        txt2: this.txt2,
        txt3: this.txt3,
        txt4: this.txt4,
        radioGroup: this.radioGroup,
        radioGroup2: this.radioGroup2,
      }
      db.collection('MyForm')
        .doc('formdata')
        .set(data)
        .then(function () {
          console.log('Document successfully written! -> MyForm')
        })
        .catch(function (error) {
          console.error('Error writing document: ', error)
        })

      // เก็บข้อมูล Input Text ใน collection MyText (มีหลาย document ข้อมูลจะเพิ่มขึ้นเรื่อย ๆ )แสดงผลออกมาที่ด้านนนอก
      const dataText = {
        txt: this.txt,
        timestamp: firebase.firestore.FieldValue.serverTimestamp(),
      }
      db.collection('MyText')
        .doc()
        .set(dataText)
        .then(function () {
          console.log('Document successfully written! -> MyText')
        })
    },
    reset() {},
  },
}
</script>

<style></style>
