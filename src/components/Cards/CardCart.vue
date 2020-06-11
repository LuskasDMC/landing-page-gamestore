<template>
   <div class="border rounded p-3">
       <h5>Carrinho</h5>
       <div v-if="!items.length">
            <div class="d-flex justify-content-center align-items-center pt-4 pb-4">
                <i id="cart" class="fas fa-cart-plus fa-5x"></i>
            </div>
            <p id="info" class="text-center">Até o momento o seu carrinho está vazio</p>
       </div>
       <div v-if="items.length">
                <div class="pt-2" v-for="(item,i) in items" :key="i">
                        <CardItems :item="item"/>
                </div>
                <div class="pt-2">
                    <p class="text-right"><b>Valor total:</b>R${{this.price}}</p>
                    <p class="text-right"><b>Valor do frete:</b>{{this.freight}}</p>
                    <div class="d-flex justify-content-end">
                        <button type="button" class="btn btn-success">Finalizar Compra</button>
                    </div>
                </div>
       </div>
   </div>
</template>

<script>
    import CardItems from './CardItems'
    export default {
        name:'cart',
        props:['items'],
        components:{
            CardItems
        },
        data(){
            return{
                price:0.00,
                freight:'Gratis'
            }
        },
        created(){
            this.price = this.items.reduce((prev,act)=>{
                    act.price = act.price.replace(',','.')
                    let value = Number(act.price)
                    value *= act.quantidade
                    return prev + value
                },0.00)
                this.price = this.price.toFixed(2)
                this.price > 250 ? this.freight = "Grátis" : this.freight = "R$10,00"
                this.price = String(this.price).replace('.',',')
            }
    }
</script>

<style>
    #cart{
        color: rgb(209, 209, 209);
    }
    #info{
        font-family: Roboto,sans-serif;
        font-weight: 600;
        color: rgb(163, 154, 154);
    }
</style>