<template>
<div class="container">
  <div class="row">
    <div id="app">
        <table-reader></table-reader>
    </div>
  </div>
</div>
  
</template>

<script>
// import xlsx from 'xlsx'
// import { readFile } from './services/excel'
import TableReader from './components/TableReader.vue'
export default {
  name: 'app',
  components: {
    TableReader
  }
}
</script>

<style lang="scss" src="assets/scss/app.scss">
</style>
