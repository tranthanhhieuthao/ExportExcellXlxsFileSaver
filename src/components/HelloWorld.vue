<template>
  <div class="hello">
<button @click="saveAsTest"> create</button>
  </div>
</template>

<script>
import { saveAs } from 'file-saver'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
    wbout: null
    }
  },
  mounted() {
  this.export()
  },
  methods: {
    export() {
      var XLSX = require('xlsx')
      var wb = XLSX.utils.book_new()
      // var wb1 = xl
      wb.props = {
         Title: "SheetJS Tutorial",
         Subject: "Test",
         Author: "Red Stapler",
         CreatedDate: new Date(2020,7,1)
      }
      wb.SheetNames.push('Test Sheet')
      var ws_data = [['hello', 'world']]
      var ws = XLSX.utils.aoa_to_sheet(ws_data)
      wb.Sheets['Test Sheet'] = ws
      this.wbout = XLSX.write(wb, {bookType:'xlsx',  type: 'binary'});
    },
    s2ab(s) {
                var buf = new ArrayBuffer(s.length); //convert s to arrayBuffer
                var view = new Uint8Array(buf);  //create uint8array as viewer
                for (var i=0; i<s.length; i++) view[i] = s.charCodeAt(i) & 0xFF; //convert to octet
                return buf;    
     },
     saveAsTest() {
        saveAs(new Blob([this.s2ab(this.wbout)], {type:"application/octet-stream"}), 'test.xlsx')
     }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
