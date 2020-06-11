<template>
    <div @mouseenter="handleAddShopOver" @mouseleave="handleAddShopOut">
        <div id="card" class="d-flex justify-content-center align-items-center rounded">
            <img :src="data.path"  id="image-games">
        </div>
        <div v-if="!addShopIsOpen">
            <span  class="text-center d-block" id="title">{{data.name}}</span>
            <span  class="text-center d-block font-weight-bold" id="price">R${{data.price}}</span>
        </div>
        <span v-if="addShopIsOpen" @click="onClickAddCart(data)" class="text-center d-block font-weight-bold" id="price">
            Adicionar ao carrinho
        </span>
    </div>

</template>

<script>
export default {
    name:'card',
    props:{
        data:Object,
        onClickAdd:Function
    },
    data(){
        return {
            addShopIsOpen:false
        }
    },
    methods:{
        handleAddShopOut(){
            this.addShopIsOpen = false
        },
        handleAddShopOver(){
            this.addShopIsOpen = true
        },
        onClickAddCart(data){
            const newData = Object.assign(data,{quantidade:1})
            this.onClickAdd(newData)
        }
    },
    created(){
        console.log(this.data)
    }
}
</script>

<style>
    #image-games{
        max-height: 180px;
        max-width: 150px;
    }
    #card{
        background-color: rgb(240, 240, 240);
        height: 220px;
        cursor: pointer;
    }
    #title{
        font-family: Roboto,sans-serif;
        font-size: 1.1rem;
        color:rgb(138, 138, 138) ;
    }
    #price{
        color: cornflowerblue;
        cursor: pointer;
    }
</style>