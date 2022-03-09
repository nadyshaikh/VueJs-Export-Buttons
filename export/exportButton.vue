<template>
    <div class="text-nowrap pl-md-2">     
                            
                            <b-button
                                v-ripple.400="'rgba(255, 255, 255, 0.15)'"
                                variant="gradient-danger"
                                class="btn-icon"
                                  title="Print"
                                  v-b-tooltip.hover.v-default
                                  @click="ExportCSV('print')"
                            >
                                <FontAwesomeIcon icon="fa-print" size="lg" />
                            </b-button>

                            <b-button
                                v-ripple.400="'rgba(255, 255, 255, 0.15)'"
                                variant="gradient-danger"
                                class="btn-icon"
                                  title="Export to Pdf"
                                  v-b-tooltip.hover.v-default
                                  @click="ExportCSV('pdf')"
                            >
                                 <FontAwesomeIcon icon="fa-file-pdf" size="lg" />
                            </b-button>
                            <b-button
                                v-ripple.400="'rgba(255, 255, 255, 0.15)'"
                                variant="gradient-warning"
                                class="btn-icon"
                                title="Export to CSV"
                                  v-b-tooltip.hover.v-default
                                  @click="ExportCSV('csv')"
                            >
                                 <FontAwesomeIcon icon="fa-file-csv" size="lg" />
                               
                            </b-button>
                            <b-button
                                v-ripple.400="'rgba(255, 255, 255, 0.15)'"
                                variant="gradient-success"
                                class="btn-icon"
                                 title="Export to Excel"
                                  v-b-tooltip.hover.v-default
                                  @click="ExportCSV('excel')"
                            >
                                  <FontAwesomeIcon icon="fa-file-excel" size="lg" />
        
                            </b-button>
                          

                        </div>
</template>

<script>
import {   
    BTooltip,
    BButton,
    VBTooltip,
} from "bootstrap-vue";
import Ripple from "vue-ripple-directive";

//import "tableexport.jquery.plugin/libs/jsPDF/polyfills.umd.min.js"
//import "tableexport.jquery.plugin/libs/jsPDF/jspdf.umd.min.js"
//import "tableexport.jquery.plugin/libs/pdfmake/pdfmake.min.js"
//import "tableexport.jquery.plugin/libs/html2canvas/html2canvas.min.js"

 import jsPDF from 'jspdf';
 import 'jspdf-autotable';
import "tableexport.jquery.plugin/tableExport.min.js"
export default {
    props : {
        fileName : {
            type: String,
                default() { 
                    return 'FBS-Busineess-suite'; 
                }          
        },
        layout : {
            type: String,
            default() { 
                    return 'p'; 
            } 
        },
        htmlElement: {
            type: String,
            default() { 
                    return 'table'; 
            } 
            
        },
        theme: {
            type: String,
            default() { 
                    return 'grid';  // Theme Type - striped, grid, plain
            } 
            
        },
        ignoreColumnArray:{
            default() { 
                    return [];  // Theme Type - striped, grid, plain
            } 
        }
    },
    components : {
        BButton,
        BTooltip,
        VBTooltip,   
             
    },   
    directives: {
        Ripple,
        'b-tooltip': VBTooltip,
        jsPDF
    },
   
    setup(props){
        //const { jsPDF } = jspdf
        //  const { jsPDF } = jsPDF
        // let print = () => {
        //     const doc = new jsPDF()
        //     doc.addHTML(jQuery('table'), 15, 15, function() {
        //         doc.print();
        //     });
        // }

        var specialElementHandlers = {
            // element with id of "bypass" - jQuery style selector
            '.no-export': function(element, renderer) {
            // true = "handled elsewhere, bypass text extraction"
            return true;
            }
        };


        console.log(props.ignoreColumnArray);

         const ExportCSV = (fileType) => {
          // console.log('fsdfs')
           

             if(fileType=='pdf')
             {                        
                 var doc = new jsPDF(props.layout, 'pt', "a4");

                    var tTB = document.getElementById(props.htmlElement);                   
                    var atTB = doc.autoTableHtmlToJson(tTB, true);                     
                    var cols = atTB.columns;
                    //here you are going to set which column you will truncate. Moreover, .splice(index number of the column(your start), the number of columns you will exclude)
                    props.ignoreColumnArray.forEach(function(fetch) {  
                        console.log(fetch); 
                        cols.splice(fetch);  
                    });  

                   
                    //var name = "Doe, John"
                    // doc.setFontType("normal");
                     doc.setFontSize(8);
                    //doc.text(20,20,'Name: '+ name);
                    //doc.autoPrint();                   

                    doc.autoTable({  
                        html: props.htmlElement,  
                        startY: 70,  
                        theme: props.theme, 
                        
                        // columnStyles: {  
                        //     0: {  
                        //         cellWidth: 180,  
                        //     },  
                        //     1: {  
                        //         cellWidth: 180,  
                        //     },  
                        //     2: {  
                        //         cellWidth: 180,  
                        //     }  
                        // },  
                        // styles: {  
                        //     minCellHeight: 10  
                        // }  
                    })  
                    doc.save(props.fileName+'.pdf'); 
                    //This is a key for printing
                    //doc.output('dataurlnewwindow');
                
             }else if(fileType=='print'){

                    // var tTB = document.getElementById("myTable");
                    // var atTB = doc.autoTableHtmlToJson(tTB, true);
                    // var cols = atTB.columns;
                    // //here you are going to set which column you will truncate. Moreover, .splice(index number of the column(your start), the number of columns you will exclude)
                    // cols.splice(4,1); 
                    // doc.text("My Test Table", 40, 60);
                    // doc.autoPrint();
                    var doc = new jsPDF(props.layout, 'pt', "a4");
                    var tTB = document.getElementById(props.htmlElement);                   
                    var atTB = doc.autoTableHtmlToJson(tTB, true);                     
                    var cols = atTB.columns;
                    //here you are going to set which column you will truncate. Moreover, .splice(index number of the column(your start), the number of columns you will exclude)
                    props.ignoreColumnArray.forEach(function(fetch) {  
                        console.log(fetch); 
                        cols.splice(fetch);  
                    });  
                   // cols.splice(8);                    
                    console.log(cols);
                    //doc.text("My Test Table", 40, 60);
                    doc.autoTable(cols, atTB.data, {theme: props.theme, margin: {top: 10}});

                    doc.autoPrint();



                 //doc.autoPrint();
                    // var doc = new jsPDF(props.layout, 'pt', "a4");
                    // //var name = "Doe, John"
                    // // doc.setFontType("normal");
                    //  doc.setFontSize(12);
                    // //doc.text(20,20,'Name: '+ name);
                    // //doc.autoPrint();
                    // doc.autoTable({  
                    //     html: props.htmlElement,  
                    //     startY: 30,
                    //     startX: 30, 
                    //     theme: props.theme,
                    //     showHead: 'firstPage',
                    //     ignoreColumn:['Contact Id', 'Name', 'Mobile','Contact Type (Click to sort Ascending)']
                    //     //head: ['Contact ID', 'Name', 'Mobile']   
                    //     // columnStyles: {  
                    //     //     0: {  
                    //     //         cellWidth: 180,  
                    //     //     },  
                    //     //     1: {  
                    //     //         cellWidth: 180,  
                    //     //     },  
                    //     //     2: {  
                    //     //         cellWidth: 180,  
                    //     //     }  
                    //     // },  
                    //     // styles: {  
                    //     //     minCellHeight: 10  
                    //     // }  
                    // })  
                    // doc.autoPrint();
                   // doc.save('Marks_Of_Students.pdf'); 
                    //This is a key for printing
                    doc.output('dataurlnewwindow');
             }else{
                    //you can use the column number in ignoreColumn syntax as displaying in the following exapmle 
                    jQuery('#'+props.htmlElement).tableExport({type:fileType, fileName:props.fileName, escape:'true',ignoreColumn: [props.ignoreColumnArray]});
                    
             }
       };

       return {
          ExportCSV
          //jsPDF
        //   print,
        //   savePDF
       }
    }
}

</script>
