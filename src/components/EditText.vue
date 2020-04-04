<template>
  <div class="wrapper">
    <div class="nav">
      <button @click="selectFile">open</button>
      <!-- <button @click="getNewFileHandle">New File</button>-->
      <button @click="writeFile">save</button>
      <button @click="saveAs">save As</button>
    </div>
    <br />
    <textarea
      class="text-area"
      name="comment"
      form="usrform"
      v-model="contents"
    />
  </div>
</template>

<script>
export default {
  name: 'EditText',
  props: {
    msg: String
  },
  data() {
    return {
      fileHandle: '',
      contents: ''
    }
  },
  created() {},
  methods: {
    async selectFile() {
      this.fileHandle = await window.chooseFileSystemEntries()
      const file = await this.fileHandle.getFile()
      this.contents = await file.text()
      console.log(file)
    },
    async writeFile() {
      // Create a writer (request permission if necessary).
      const writer = await this.fileHandle.createWriter()
      // Write the full length of the contents
      await writer.write(0, this.contents)
      // Close the file and write the contents to disk
      await writer.close()
      this.contents = ''
    },
    async getNewFileHandle() {
      const opts = {
        type: 'saveFile',
        text: 'hola',
        accepts: [
          {
            description: 'Text file',
            extensions: ['txt'],
            mimeTypes: ['text/plain']
          }
        ]
      }
      const handle = await window.chooseFileSystemEntries(opts)
      return handle
    },
    async saveAs() {
      const file = await this.getNewFileHandle()
      const writer = await file.createWriter()
      // Write the full length of the contents
      await writer.write(0, this.contents)
      // Close the file and write the contents to disk
      await writer.close()
      this.contents = ''
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.wrapper {
  width: 100vw;
  height: 100vh;
  .nav {
    background-color: #4DBA87;
    button {
      background-color: #4DBA87;
      border: none;
      margin: 1rem;
      color: black;
    }
  }
}
.text-area {
  width: 99%;
  height: px;
}
</style>
