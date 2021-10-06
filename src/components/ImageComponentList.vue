<template>

    <ul>
    <li v-for="imageNum in images" :key="imageNum">  
        <image-component
            :imageName="imageNum" 
            @toggled-image="handleToggled"
            @submit-caption="submitCaption" 
        >
        </image-component> 
    </li>
</ul>

</template>

<script>
import ImageComponent from './ImageComponent.vue';

export default {
    components: {ImageComponent},
    emits: ['alter-selected', 'submit-caption'], 
    props: ["images"],
    methods:{
        handleToggled(selected, imageNum){
            this.$emit('alter-selected', selected, imageNum);  
        }, 
        submitCaption(imageName, caption ){
            this.$emit('submit-caption', imageName, caption); 
        }
    }
}
</script>

<style scoped>
ul {
  padding-top: 7%;
  padding-bottom: 10%; 
  width: 40%; 
  list-style-type: none;
  padding-left: 30%;
}

</style>
