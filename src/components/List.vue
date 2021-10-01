<template>
    <div class="list-content">
        <h2>Your tasks</h2>
        <div class="list-main">
            <ul>
                <li v-for="(taskText,n) in tasks" :key="taskText.id">
                    <div class="title"> {{ tasks[n].taskText }} </div>
                    <div class="date">{{ tasks[n].date }} </div>
                    <button class="done" @click="removeTasks(tasks[n].id)">DONE</button>
                    <button class="edit" @click="selectedId(tasks[n].id)">EDIT</button>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
    name:'List',
    props:['tasks'],
    data (){
        return{
        }
    },
    methods:{
        removeTasks(n){
            this.$emit('removeTask', n);
            this.deleteItemFromBackend(n);
        },
        selectedId(id){
            this.$emit('selectedId', id);
        },
        deleteItemFromBackend(id){
           fetch(`http://localhost:5500/todos/${id}`, {
                method: "DELETE",
            });
        }
    }
}
</script>
<style lang="scss">
@import '@/assets/_shared.scss';

.list-content{
    @include setSize(90%, 55%);
    @include display(flex, center, center, column);
    background-color: $secondColor ;
    border-top-left-radius: 30px;
    border-top-right-radius: 30px;
    box-shadow: 2px 2px 5px $secondColor;
}
h2{
    @include setSize(15%, 100%);
    @include display(flex, center, center, row);
    font-size: 26px;
    font-family: 'Montserrat Alternates', sans-serif;
    color: $bckColor;
    padding: 10px;
    text-decoration: underline $firstColor;
}

.list-main{
    @include setSize(85%, 100%);
    @include display(flex,center,center,row);
    background-color: $bckColor;
}


ul{
    @include setSize(100%,100%);
    list-style-type: none;
    overflow-y: auto;
}
ul::-webkit-scrollbar{
    width: 10px;
    border-radius: 30px
}

ul::-webkit-scrollbar-track {
  background: white;
  border-radius: 30px;
}

ul::-webkit-scrollbar-thumb {
  background: $fontColor;
  border-radius: 30px
}

li{
    @include setSize(50px, 100%);
    @include display(flex, space-between,center,row);
    margin-bottom: 20px;
    border-bottom: 3px solid $firstColor;
    border-top: 3px solid $firstColor;
    font-size: 18px;
    font-family: 'Montserrat Alternates', sans-serif;

    button{
        @include setSize(80%, 10%);
        background-color: $fontColor;
        border: none;
        color: $bckColor;
        cursor: pointer;
        margin-right: 10px;
        border-radius: 10px;
        &:hover{
            background-color: $secondColor;
            color: $bckColor;
        }
    }
}
.title{
    @include setSize(100%, 50%);
    @include display(flex,flex-start,center,row);
    padding: 10px;
}

.date{
    @include setSize(100%,30%);
    @include display(flex,center,center,row);
}

@media only screen and (max-width: 1040px){
    .list-content{
        @include setSize(90%, 60%);
    }
}

@media only screen and (max-width: 780px){

    h2{
        background-color: $bckColor;
        color: $fontColor;
        text-decoration: underline $secondColor;
    }

    .list-content{
        @include setSize(80%, 100%);
        border-radius: 0;
    }
}

@media only screen and (max-width: 450px){
    .title{
        font-size: 14px !important;
        padding: 5px;
    }

    .date{
        font-size: 14px !important;
        width: 20%;
    }

    li{
        button{
            font-size: 12px !important;
        }
    }
}
@media only screen and (max-width: 360px){
    .title{
        font-size: 12px !important;
    }

    .date{
        font-size: 10px !important;
    }

    li{
        button{
            height: 25px !important;
            width: 25px !important;
            border-radius: 5px;
            font-size: 8px !important;
        }
    }
}
</style>