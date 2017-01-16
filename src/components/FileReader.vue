<template>
  <div>
    <form>
      <div class="form-group">
        <label for="file" class="control-label"></label>
        <input type="file" @change="fileLoad">
      </div>
      
      <div class="btn-group">
        <a href="#" @click="previewTable" class="btn btn-info">Preview Table</a>
        <a href="#" @click="previewJson" class="btn btn-primary">Preview Json</a>
      </div>

      <div v-show="isProcessing">
        <span>Processing.........</span>
      </div>

      

      <input type="hidden" :value="selectedSheetName">

      <div v-show="showJson">
        <div class="form-group">
          <label for="sheet_name">Choose Sheet Name</label>
          <select id="sheet_name" v-model="selectedSheetName" required="true" @change="updateJsonString" class="form-control">
            <option v-for="(sheetData, sheetName) in sheets">{{ sheetName }}</option>
          </select>
        </div>
        <h4>Json preview of the sheet</h4>
        <pre style="text-align: left">
          <code>{{ json_string }}</code>
        </pre>
      </div>

      <div v-show="showPreview">

        <table class="table table-bordered" v-for="(sheetData, sheetName) in sheets">
          <thead>
            <tr>
              <th>{{ sheetName }}</th>
            </tr>
          </thead>
          <tr v-for="row in sheetData.data">
            <td v-for="col in row" v-html="col"></td>
          </tr>
        </table>
      </div>
    </form>

    
  </div>
</template>
<script>
  import { readFile } from '../services/excel'
  export default {
    data () {
      return {
        excelFile: '',
        showJson: false,
        showPreview: false,
        isProcessing: false,
        sheets: [],
        selectedSheetName: '',
        json_string: ''
      }
    },
    updated () {
      console.log(this.sheets)
    },
    methods: {
      fileLoad (e) {
        this.excelFile = e.target.files[0]
      },
      previewTable () {
        this.isProcessing = true
        this.sheets = []
        this.showJson = false
        this.showPreview = true
        if (this.excelFile) {
          readFile(this.excelFile, this.showPreview, this.showJson).then((data) => {
            this.sheets = data.sheets
            this.isProcessing = false
          })
        }
      },
      previewJson () {
        this.isProcessing = true
        this.sheets = []
        this.showJson = true
        this.showPreview = false
        if (this.excelFile) {
          readFile(this.excelFile, this.showPreview, this.showJson).then((data) => {
            this.sheets = data.sheets
            this.json_string = this.sheets
            this.isProcessing = false
          })
        }
      },
      updateJsonString () {
        this.json_string = JSON.stringify(this.sheets[this.selectedSheetName], null, 2)
      }
    }
  }
</script>
