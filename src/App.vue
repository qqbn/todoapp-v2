<template>
    <div id="main">
      <Header/>
      <Content :tasks="tasks" @addTask="addTask($event)" @removeTask="removeTask($event)" @editedTask="editedTask($event)" @selectedId="selectedId($event)" :idToEdit="idToEdit"/>
    </div>
</template>

<script>
import Header from'./components/Header.vue'
import Content from './components/Content.vue'

export default {
  name: 'App',
  components:{
    Header,
    Content,
  },
  data(){
      return{
          tasks:[],
          idToEdit: null,
      }
  },
  methods:{
        getItemsFromBackend(){
            fetch('http://localhost:5500/todos')
            .then(res => res.json())
            .then(data => this.setStartingData(data))
            .catch(err => console.log(err.message))
        },

        setStartingData(data){
            data.forEach(element => {
                this.tasks.push(element);
            });
        },

        addTask(n){
            this.tasks.push(n);
        },
        removeTask(n){
            const taskId=n;
            const index = this.tasks.findIndex(element => element.id === taskId)
            this.tasks.forEach(element => {
                if(element.id===n){
                    this.tasks.splice(index,1);
                }
            });
        },
        editedTask(n){
            this.tasks.forEach(element => {
                if(element.id===n.id){
                    element.taskText=n.taskText,
                    element.date=n.date
                }
            });
        },
        selectedId(n){
            this.idToEdit=n;
        },
  },
  mounted(){
    this.getItemsFromBackend();
  }
}
</script>

<style lang="scss">
@import '@/assets/_shared.scss';
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
#main{
    @include setSize(100vh,100%);
}
</style>
