<template>
  <div id="app">
      <div class="form-group">
        <label for='file' class="control-label">文件</label>
        <input type="file" @change="fileLoad">
      </div>
      <!-- {{ sheets }} -->
      <!-- <ul>
        <li v-for="sheet in sheets">{{ sheet }}</li>
      </ul> -->
      <!-- <ul>
        <li v-for="(sheetData, sheetName) in sheets">{{ sheetName }}:{{ sheetData }}</li>
      </ul> -->
      <table v-for="(sheetData, sheetName) in sheets">
        <thead>
          <tr>
            <th>{{ sheetName }}</th>
          </tr>
        </thead>
        <tr v-for="row in sheetData.data">
          <td v-for="col in row">{{ col }}</td>
        </tr>
      </table>

    </div>
</template>

<script>
// import xlsx from 'xlsx'
import { readFile } from './services/excel'
export default {
  name: 'app',
  data () {
    return {
      showJsonPreview: false,
      selectedSheetName: '',
      isProcessing: false,
      sheets: null,
      json_string: []
    }
  },
  updated () {
    console.log(this.sheets)
  },
  methods: {
    fileLoad (e) {
      let file = e.target.files[0]
      readFile(file, !this.showJsonPreview, this.showJsonPreview).then((data) => {
        this.sheets = data.sheets
      })
    },
    showPreview () {
      this.showJsonPreview = !this.showJsonPreview
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
