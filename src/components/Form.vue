<template>
    <div class="form-content">
        <div class="form-header">
            <h3>Add new task!</h3>
        </div>
        <div class="form-main">
            <div class="form">
                <input type="text" class="input" placeholder="Whats need to be done?" v-model="newTaskText">
                <input type="date" class="input" v-model="newDate">
                <button class="add-edit"  id="btn1" @click="addTask" v-if="!isEdit">Add!</button>
                <button class="add-edit" id="btn2" @click="updateTasks" v-if="isEdit">Edit!</button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'Form',
    props:{
        idToEdit: Number,
        tasks: Array,
    },
     data() {
            return {
                newTask: [],
                newTaskText: null,
                newDate: null,
                isEdit: false,
                taskId: null,
                editedTask: [],
            }
    },
    watch: {
        idToEdit: function (newId) {
        this.editTask(newId);
        },
    },
    methods: {
        addTask(){
            if(this.validateForms()){
                this.newTask=[];
                this.newTask.push({
                    id: Math.floor(Math.random() * 100000),
                    taskText: this.newTaskText,
                    date: this.newDate,
                });
                this.sendItemToBackEnd(this.newTask[0]);
                this.newTaskText='';
                this.newDate='';
                this.$emit('addTask',this.newTask[0]);
            }else{
                // this.newTaskText='';
                // this.newDate='';
            }
        },
        editTask(n){
            this.isEdit=true;
            this.taskId=n;
            let selectedTask;
            this.tasks.forEach((element)=>{
                if(element.id===n){
                    selectedTask=element;
                }
            });
            this.newTaskText=selectedTask.taskText;
            this.newDate=selectedTask.date;
        },
        updateTasks(){
            this.editedTask=[];
            this.editedTask.push({
                id: this.taskId,
                taskText: this.newTaskText,
                date: this.newDate,
            });
            this.updateTasksInBackend(this.editedTask[0]);
            this.isEdit=false;
            this.newTaskText='';
            this.newDate='';
            this.$emit('editedTask',this.editedTask[0]);
        },
        sendItemToBackEnd(item){
            fetch('http://localhost:5500/todos', {
                method: "POST",
                body: JSON.stringify(item),
                headers: {
                    "Content-type": "application/json"
                }
            });
        },
        updateTasksInBackend(update){
            return fetch(`http://localhost:5500/todos/${update.id}`,{
                method: "PATCH",
                body: JSON.stringify(update),
                headers: {
                    "Content-type": "application/json"
                }
            })
        },
        validateForms(){
            if(this.newTaskText==null || this.newTaskText==undefined || this.newTaskText==''){
                confirm('Set title!');
                return false;
            }else if(this.newTaskText.length > 20){
                confirm('Title is too long!')
                return false;
            }

            if(this.newDate==null || this.newDate==undefined || this.newDate==''){
                confirm('Set date!')
                return false
            }

            return true;
        }
    },
}
</script>

<style lang="scss">
@import '@/assets/_shared.scss';
.form-content{
    @include setSize(60%, 35%);
    @include display(flex,center,center,column);
    border-top-left-radius: 30px;
    border-top-right-radius: 30px;
    box-shadow: 2px 2px 5px $secondColor;
}

.form-header{
    @include setSize(20%,100%);
    background-color: $secondColor;
    @include display(flex,center,center,row);
    font-family: 'Montserrat Alternates', sans-serif;
    color: $bckColor;
    border-top-left-radius: 30px;
    border-top-right-radius: 30px;
    font-size: 20px;
    text-decoration: underline $firstColor;
}
.form-main{
    @include setSize(80%, 100%);
    @include display(flex,space-around,center,column);

    .form{
        @include setSize(100%, 100%);
        @include display(flex,space-around,center,column);
        input[type=text]{
            @include setSize(50px, 60%);
            font-size: 18px;
            font-family: 'Montserrat Alternates', sans-serif;
            border: 2px solid $firstColor;
            cursor: pointer;
            color: $fontColor;
        }

        input[type=date]{
            @include setSize(50px, 60%);
            font-size: 18px;
            font-family: 'Montserrat Alternates', sans-serif;
            border: 2px solid $firstColor;
            cursor: pointer;
            color: $fontColor;
        }

        .add-edit{
            @include setSize(50px, 100px);
            border: none;
            background-color: $fontColor;
            color: $bckColor;
            border-radius: 10px;
            font-size: 18px;
            font-family: 'Montserrat Alternates', sans-serif;
            cursor: pointer;
            &:hover{
                background-color: $secondColor;
            }
        }
    }
}

@media only screen and (max-width: 1020px){

    input[type=date], input[type=text]{
        width: 80% !important;
    }

}

@media only screen and (max-width: 830px){
    input[type=date], input[type=text]{
        font-size: 16px !important;
    }
}

@media only screen and (max-width: 780px){
    .form-header{
        border-radius: 0;
    }
    .form-content{
        @include setSize(20%, 100%);
    }
    .form-main{
        @include display(flex, center, center, row);
    }

    .form{
        flex-direction: row !important;
    }

    input[type=date], input[type=text]{
        width: 40% !important;
    }
}
@media only screen and (max-width: 520px){
    input[type=date], input[type=text]{
        width: 30% !important;
        font-size: 12px !important;
    }

    .add-edit{
        width: 50px !important;
        height: 50px !important;
        font-size: 16px !important;
    }
}
</style>