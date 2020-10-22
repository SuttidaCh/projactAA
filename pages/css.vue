<template>
  <v-container>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-text-field
        v-model="name"
        :counter="10"
        :rules="nameRules"
        label="Name"
        required
      ></v-text-field>
      <v-text-field
        v-model="id"
        type="number"
        :rules="idRules"
        label="ID"
        required
      ></v-text-field>
      <v-menu
        v-model="menu"
        hide-details="auto"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        min-width="290px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-text-field
            v-model="date"
            hide-details="auto"
            label="Date"
            prepend-icon="mdi-calendar"
            readonly
            v-bind="attrs"
            required
            filled
            v-on="on"
          />
        </template>
        <v-date-picker v-model="date" @input="menu = false" />
      </v-menu>
      <br />
      <v-text-field
        v-model="address"
        :rules="addressRules"
        label="Address"
        required
      ></v-text-field>
      <br />
      <template>
        <v-file-input
          v-model="fileImage"
          accept="image/*"
          label="File input"
        ></v-file-input>
      </template>
      <br />
      <v-btn color="success" class="mr-4" @click="submitform">Submit</v-btn>
      <v-btn color="warning" class="mr-4" @click="reset"> Reset Form </v-btn>
    </v-form>
    <br />
    <v-progress-circular
      :rotate="-90"
      :size="100"
      :width="15"
      :value="value"
      color="primary"
    >
      {{ value }}
    </v-progress-circular>
  </v-container>
</template>
<script>
import firebase from 'firebase/app'
export default {
  data() {
    return {
      name: '',
      id: '',
      address: '',
      fileImage: null,
      date: new Date().toISOString().substr(0, 10),
      menu: false,
      value: 0,
    }
  },
  methods: {
    submitform() {
      // เก็บข้อมูล --> cloud storage
      // เก็บไฟล์ --> storage
      // Create a root reference
      const storageRef = firebase.storage().ref()
      // File or Blob named mountains.jpg
      const file = this.fileImage
      // Create the file metadata
      const metadata = {
        contentType: 'image/jpeg',
      }
      // Upload file and metadata to the object 'images/mountains.jpg'
      const uploadTask = storageRef
        .child('images/' + this.id)
        .put(file, metadata)
      // Listen for state changes, errors, and completion of the upload.
      uploadTask.on(
        firebase.storage.TaskEvent.STATE_CHANGED, // or 'state_changed'
        (snapshot) => {
          // Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
          const progress =
            (snapshot.bytesTransferred / snapshot.totalBytes) * 100
          console.log('Upload is ' + progress + '% done')
          this.value = progress
          switch (snapshot.state) {
            case firebase.storage.TaskState.PAUSED: // or 'paused'
              console.log('Upload is paused')
              break
            case firebase.storage.TaskState.RUNNING: // or 'running'
              console.log('Upload is running')
              break
          }
        },
        (error) => {
          // A full list of error codes is available at
          // https://firebase.google.com/docs/storage/web/handle-errors
          switch (error.code) {
            case 'storage/unauthorized':
              // User doesn't have permission to access the object
              break
            case 'storage/canceled':
              // User canceled the upload
              break
            case 'storage/unknown':
              // Unknown error occurred, inspect error.serverResponse
              break
          }
        },
        () => {
          // Upload completed successfully, now we can get the download URL
          uploadTask.snapshot.ref.getDownloadURL().then((downloadURL) => {
            console.log('File available at', downloadURL)
          })
        }
      )
    },
  },
}
</script>
