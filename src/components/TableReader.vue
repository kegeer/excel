<template>
  <div>
    <form>
      <div class="form-group">
        <label for="file" class="control-label"></label>
        <input type="file" @change="fileLoad">
      </div>

      <a href="#" @click="previewTable" class="btn btn-info">预览表格</a>
      <a href="#" @click="saveTable" v-show="selectedRow.length > 0" class="btn btn-primary">保存到数据库</a>

      <div v-show="isProcessing">
        <span>Processing.........</span>
      </div>

      <div v-show="showPreview">

        <ul class="list-group">
          <li  v-for="(sheetData, sheetName) in sheets" class="list-group-item" v-html="sheetName" @click="selectedSheet(sheetName)" style="display:inline-block;cursor:pointer;">
          </li>
        </ul>
        <span v-show="!sheetData.data">
          Empty table
        </span>
        <table class="table table-bordered">
          <tr v-for="(row, index) in sheetData.data" class="text-center">
            <td>
              <input type="checkbox" @change="selectRow(index)">
            </td>
            <td v-for="(col, index) in row" v-html="col"></td>
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
        showPreview: false,
        isProcessing: false,
        sheets: [],
        json_string: '',
        sheetData: {},
        selectedRow: []
      }
    },
    updated () {
      // console.log(this.sheets)
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
          readFile(this.excelFile, this.showPreview, false).then((data) => {
            this.sheets = data.sheets
            this.isProcessing = false
          })
        }
      },
      selectedSheet (sheetName) {
        this.sheetData = this.sheets[sheetName]
        console.log(this.sheetData)
      },
      selectRow (index) {
        // if (index === 0) {
        //   this.selectedRow.push(this.sheetData.data)
        // }
        // // console.log(index)
        this.selectedRow.push(this.sheetData.data[index])
        console.log(this.selectedRow)
      },
      saveTable () {
        // this.selectedRow.forEach(row => this.$http.post('http://localhost:8000/api/experiments', row))
        this.$http.post('http://localhost:8000/api/experiments', {'name': 'fond'})
      }
    }
  }
</script>
<style>
  td {
    font-weight: 300;
    text-align: center;
    padding: 2px 6px;
    border:1px solid #ededed;
    word-break: keep-all;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>
