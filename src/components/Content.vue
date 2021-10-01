<template>
    <div class="content">
        <List :tasks="tasks" @selectedId="selectedId($event)" @removeTask="removeTask($event)"/>
        <Form :tasks="tasks" @addTask="addTask($event)" @editedTask="editedTask($event)" :idToEdit="idToEdit"/>
    </div>
</template>
<script>
import List from './List.vue'
import Form from './Form.vue'
export default {
   name: 'Content',
   data(){
       return{
       }
   },
   components:{
       Form,
       List,
   },
   props:{
       tasks: Array,
       idToEdit: Number,
   },
    methods: {
       selectedId(n){
           this.$emit('selectedId', n);
       },

       addTask(n){
           this.$emit('addTask', n);
       },

       editedTask(n){
           this.$emit('editedTask', n);
       },
       removeTask(n){
           this.$emit('removeTask', n);
       }
   },
   mounted(){
    },
}
</script>
<style lang="scss">
    @import '@/assets/_shared.scss';
    .content{
        @include setSize(80%,100%);
        @include display(flex,space-around,center,row);
    }

@media only screen and (max-width: 780px){
    .content{
        @include display(flex,center,center,column);
        @include setSize(90%, 100%);
    }
}
</style>