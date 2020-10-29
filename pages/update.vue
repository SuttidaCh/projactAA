<template>
  <v-container>
    <v-form>
      <h1>Update Status</h1>
      <br />
    </v-form>
    <v-data-table :headers="headers" :items="textList">
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>อัปเดตสถานะพัสดุ</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <v-card>
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.status"
                        label="สถานะ"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      </template>
    </v-data-table>
  </v-container>
</template>
<script>
import firebase from 'firebase/app'
import { db } from '~/plugins/firebaseConfig.js'
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'หมายเลขติดตามพัสดุ',
        value: 'track',
        align: 'start',
        sortable: false,
      },
      { text: 'ชื่อ-สกุล', value: 're_name' },
      { text: 'ที่อยู่', value: 're_address' },
      { text: 'ตำบล/อำเภอ/จังหวัด/รหัสไปรษณีย์', value: 're_exstates' },
      { text: 'วันที่ทำรายการ', value: 'date' },
      { text: 'สถานะ', value: 'status' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
    textList: [],
    editedIndex: -1,
    editedItem: {
      status: '',
    },
    defaultItem: {
      status: '',
    },
  }),

  watch: {
    dialog(val) {
      val || this.close()
    },
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
      db.collection('Recipient')
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

  editItem(item) {
    this.editedIndex = this.textList.indexOf(item)
    this.editedItem = Object.assign({}, item)
    this.dialog = true
  },

  close() {
    this.dialog = false
    this.$nextTick(() => {
      this.editedItem = Object.assign({}, this.defaultItem)
      this.editedIndex = -1
    })
  },

  save() {
    if (this.editedIndex > -1) {
      Object.assign(this.textList[this.editedIndex], this.editedItem)
    } else {
      this.textList.push(this.editedItem)
    }
    this.close()
  },
}
</script>
