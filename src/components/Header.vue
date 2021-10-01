<template>
        <div class="header">
            <div class="header-box"><h1> toDoapp </h1></div>
        </div>
</template>
<script>
export default {
    name:'Header',
}
</script>
<style lang="scss">
@import '@/assets/_shared.scss';
.header{
    @include display(flex, center, center, row);
    height: 20%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: flex-start;
}
.header-box{
    background-color: $secondColor;
    @include setSize(100px, 50%);
    @include display(flex, center, center, row);
    border-bottom-left-radius: 40px;
    border-bottom-right-radius: 40px;
}
h1{
    color: $bckColor;
    text-decoration: underline $firstColor;
    text-align: center;
    padding: 20px;
    font-family: 'Montserrat Alternates', sans-serif;
}

@media only screen and (max-width: 810px){
    h1{
        font-size: 26px;
    }
}
@media only screen and (max-width: 780px){
    .header{
        height: 10% !important;
    }

    .header-box{
        height: 100%;
        width: 100% !important;
    }
}

@media only screen and (max-width: 660px){
    h1{
        font-size: 22px;
    }
}
@media only screen and (max-width: 580px){
    .header-box{
        display: flex;
        justify-content: center;
        align-items: center;
    }
    h1{
        text-align: center;
    }
}

</style>