<template>

    <input type="checkbox" :id="imageName" @change="toggleSelected"/>
    <label :for="imageName">
        <img :src="require(`/public/images/${imageName}.jpeg`)" width="240" height="200"> 
    </label>
    <div v-if="selected">
      <input type="text" v-model="caption" placeholder="enter caption">
      <button @click="submitCaption">Submit Caption</button> 
    </div> 

</template>
<script>
export default {
    emits:['toggled-image', 'submit-caption'], 
    props: ['imageName'],
    data(){
        return{
            selected: false,
            caption: '',
        }; 
    },
    watch:{
      selected(){
        if(!this.selected){
          this.caption = ''; 
        }
      }
    }, 
    methods:{
        toggleSelected(){
            this.selected = !this.selected;
            this.$emit('toggled-image', this.selected, this.imageName);
        },
        submitCaption(){
          this.$emit('submit-caption', this.imageName, this.caption); 

        }
    }
} 
</script>

<style scoped>

input[type=text] {
  width: 60%;
  padding: 12px 20px;
  margin: 8px 0;
  box-sizing: border-box;
}
/* css taken from: https://codepen.io/echosoft/pen/RaVEvG */

input[type="checkbox"]{
  display: none;
}

label {
  border: 1px solid #fff;
  padding: 10px;
  display: block;
  position: relative;
  margin: 10px;
  cursor: pointer;
}

label:before {
  background-color: white;
  color: white;
  content: " ";
  display: block;
  border-radius: 50%;
  border: 1px solid grey;
  position: absolute;
  top: -5px;
  left: -5px;
  width: 25px;
  height: 25px;
  text-align: center;
  line-height: 28px;
  transition-duration: 0.4s;
  transform: scale(0);
}

label img {
  /* height: 100px;
  width: 100px; */
  transition-duration: 0.2s;
  transform-origin: 50% 50%;
}

:checked + label {
  border-color: #ddd;
}

:checked + label:before {
  content: "✓";
  background-color: blue;
  transform: scale(1);
}

:checked + label img {
  transform: scale(0.9);
  /* box-shadow: 0 0 5px #333; */
  z-index: -1;
}
</style>