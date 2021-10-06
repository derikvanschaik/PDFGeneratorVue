<template>
  
  <the-header 
      title="Pdf Generator" 
      :selected="headerOutput" 
      :selectedCount="selectedCount" 
      @make-pdf="makePdf"> 
  </the-header>

  <image-component-list 
      :images="images" 
      @alter-selected="alterSelected" 
      @submit-caption="alterCaption">
  </image-component-list>

</template>

<script>
import ImageComponentList from './components/ImageComponentList.vue';
import TheHeader from './components/TheHeader.vue';
import { jsPDF } from "jspdf";

export default {
  components: { ImageComponentList, TheHeader },
  data(){
    return{
      images: [],
      selectedCount: 0,
      selected: [],
      headerOutput: '', 
    } 
  },
  watch:{
    selectedCount(){
      this.headerOutput = `${this.selectedCount} selected.`; 
    }
  }, 
  created(){
    for (let i = 1; i <= 7; i++){
      this.images.push(i); 
      }
    }, 
  methods:{
    alterSelected(selected, imageNum){
      if (!selected){
        this.selectedCount--; 
        return this.selected = this.selected.filter(selected => selected.num !== imageNum); 
      }
      this.selected.push({num: imageNum }); 
      this.selectedCount++; 
    },
    alterCaption(imageNum, caption){
      const imageObj = this.selected.find(img => img.num === imageNum);
      imageObj.caption = caption; // create and assign caption property 
    },
    getDataUrl(imgSrc){ 
      // Create canvas
      const img = new Image();
      img.src = imgSrc; 
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d');
      // Set width and height 
      canvas.width = img.width;
      canvas.height = img.height;
      // Draw the image
      ctx.drawImage(img, 0, 0);
      return canvas.toDataURL('image/jpeg');
    },
    // a lot of magic numbers: just had to play with what works as text and image aren't using same units. 
    makePdf(){

      const doc = new jsPDF(); 
      const picSources = this.selected.map(num => require(`../public/images/${num.num}.jpeg`) );
      const base64s = picSources.map( picSrc => this.getDataUrl(picSrc) );
      const centre_x = 60;
      const starting_y = 10; 
      const delta_y = 50 + 7; // height of each image plus padding
      const imagesPerPage = 5;
      base64s.forEach((base64, index) =>{

        if ((index)%imagesPerPage==0 && index!==0){ 
          doc.addPage(); 
        }
        const offset =delta_y*(index%(imagesPerPage));
        const y = starting_y + offset; 
        doc.addImage(base64, 'JPEG', centre_x, y, 80, 50);
        // will be undefined if use never submitted a caption 
        if (this.selected[index].caption){
          doc.text(this.selected[index].caption, centre_x, y + 55);
        } 
      });

      doc.save('yourpdf.pdf');
      location.reload(); // quick hack to reset the page 
    }
  }
} 
</script> 

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; 
}
button {
  background-color: orange; 
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}

</style>
