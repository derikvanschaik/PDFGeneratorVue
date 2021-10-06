<template>
    <div class="header">
        <h1>{{title}}</h1>
        <p>{{selected}}</p>
        <button v-if="selectedCount > 0" @click="handleSubmit" >Generate Pdf</button> 
    </div>
</template>

<script>
export default {
    emits: ['make-pdf'], 
    props: ['title', 'selected', 'selectedCount'], 
    methods:{
        handleSubmit(){
            this.$emit('make-pdf'); 
        }
    }
}
</script>

<style scoped>
.header {
  position:fixed; /* fixing the position takes it out of html flow - knows
                   nothing about where to locate itself except by browser
                   coordinates */
  left:0;           /* top left corner should start at leftmost spot */
  top:0;            /* top left corner should start at topmost spot */
  width:100vw;      /* take up the full browser width */
  z-index:200;  /* high z index so other content scrolls underneath */
  height:150px;     /* define height for content */
  background-color:aliceblue;
}
</style>
