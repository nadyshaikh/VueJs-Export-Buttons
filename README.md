# VueJs-Export-Buttons
You can export any HTML table as PDF, CSV, XLS and can print


You need install jsPDF and autoTable.


//Export components
import exportButton from '@/views/components/export/ExportButton.vue'



export default {
  components: {
     exportButton //Export components
  },
  
}



<!-- Export button component -->
<!-- ignoreColumnArray - Count columns from 0 -->
<!-- fileName - Download File name -->
<!-- htmlElement - Table to export -->
<!-- layout - Page Layout - l for Landscape, p for portrait -->
<exportButton 
  fileName="ContactList" 
  layout="p" 
  htmlElement="contactList" 
  v-bind:ignoreColumnArray="[8]"
/>
