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
    <v-data-table :headers="headers" :items="textList" class="elevation-1">
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>อัปเดตสถานะพัสดุ</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.name"
                        label="Status"
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
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="headline"
                >Are you sure you want to delete this item?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >Cancel</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
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
        value: 'tracking',
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
      name: '',
    },
    defaultItem: {
      name: '',
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    },
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
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

  editItem(item) {
    this.editedIndex = this.textList.indexOf(item)
    this.editedItem = Object.assign({}, item)
    this.dialog = true
  },

  deleteItem(item) {
    this.editedIndex = this.textList.indexOf(item)
    this.editedItem = Object.assign({}, item)
    this.dialogDelete = true
  },

  deleteItemConfirm() {
    this.textList.splice(this.editedIndex, 1)
    this.closeDelete()
  },

  close() {
    this.dialog = false
    this.$nextTick(() => {
      this.editedItem = Object.assign({}, this.defaultItem)
      this.editedIndex = -1
    })
  },

  closeDelete() {
    this.dialogDelete = false
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
