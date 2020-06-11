<template>
    <div class="row border rounded m-0">
            <div 
                @click="handleRemoveItem" 
                class="col-2 d-flex align-items-center justify-content-center w-100 button-exclude" 
            >
                <i class="far fa-trash-alt"></i>
            </div>
        <div class="col-6 col-sm-6 col-lg-6 text-truncate p-0" id="title">
            {{item.name}}
        </div>
        <div class="col-4 col-md-6 col-lg-4 d-flex justify-content-between align-items-center">
            <i  class="fas fa-minus button-operator" @click="handleSubAmount(item.codigoProduto)"></i>
            {{item.quantidade}}
            <i class="fas fa-plus button-operator" @click="handleAddAmount(item.codigoProduto)"></i>
        </div>
    </div>

</template>

<script>
export default {
    name:'CardItems',
    props:{
        item:Object,
    },
    data(){
        return {
            addShopIsOpen:false
        }
    },
    inject:['handleAddAmount','handleSubAmount','handleRemoveItem'],
    methods:{
       
    },
    watch:{
        item(){
            console.log(this.item)
        }
    },
    created(){
        console.log(this.item)
    }
}
</script>

<style>
    .button-exclude{
        background-color: rgb(230, 230, 230);
        color: black;
        cursor: pointer;
    }
    .button-exclude:hover{
        background-color: rgb(214, 212, 212);
    }
    .button-operator{
        cursor: pointer;
    }
    #title{
        overflow: hidden;
    }

</style>