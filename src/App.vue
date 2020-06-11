<template>
  <div id="app" class="row justify-content-center m-0">
    <div class="col-11">
      <div class="row pt-5">
        <div class="col-12 col-lg-9">
          <div class="row d-flex justify-content-center justify-content-sm-between pb-4">
            <div class="col-12 col-sm-5">
            <h1 id="topic" class="text-center text-sm-left">Games</h1>
            </div>
            <div class="col-9 col-sm-5 col-lg-3 d-flex align-items-center justify-content-center">
              <Select 
                :options="options" 
                :sortByBiggestPrice="sortByBiggestPrice"
                :sortBySmallPrice="sortBySmallPrice"  
              />
              <div id="button-mobile-card" class="d-block d-lg-none d-flex fa-2x pl-4">
                  <i class="fas fa-cart-arrow-down" @click="handleToggleCart"></i>
                  <span>{{selectedsItems.length}}</span>
              </div>
              <div v-if='cartMobileIsOpen' id="cart-mobile" class="position-absolute d-block d-lg-none">
                  <CardCart :items="selectedsItems" :key="update" />
              </div>
            </div>

          </div>
          <div class="row justify-content-between">
              <div class="col-6 col-sm-6 col-md-4 pl-0 pb-5"  v-for="(game,i) in games" :key="i">
                <Card :data="game" v-bind:onClickAdd="onClickAdd"/>
              </div>
          </div>
        </div>
        <div class="d-none d-lg-block col-3">
          <CardCart :items="selectedsItems" :key="update" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Select from "./components/Selects/Select"
  import Card from "./components/Cards/Card"
  import CardCart from './components/Cards/CardCart'
  import games from './gamesMock.json'

  export default {
    name: 'App',
    components: {
      Select,
      Card,
      CardCart
    },
    methods:{
      onClickAdd(data){
        let index = this.selectedsItems.findIndex(a=>a.codigoProduto == data.codigoProduto)
        if(index == -1) this.selectedsItems.push(data)
      },
      handleAddAmount(code){
        let index = this.selectedsItems.findIndex(a=>a.codigoProduto == code)
        this.selectedsItems[index].quantidade++
        this.update++
      },
      handleSubAmount(code){
        let index = this.selectedsItems.findIndex(a=>a.codigoProduto == code)
        if(this.selectedsItems[index].quantidade > 1){
          this.selectedsItems[index].quantidade-=1
          this.update++
        }
      },
      handleRemoveItem(code){
        let index = this.selectedsItems.findIndex(a=>a.codigoProduto == code)
        this.selectedsItems.splice(index,1)
      },
      sortByBiggestPrice(){
        this.games = this.games.sort(function(a, b){
            a.price = a.price.replace(',','.')
            b.price = b.price.replace(',','.')
            return Number(a.price) == Number(b.price) ? 0 : +(Number(a.price) < Number(b.price)) || -1;
          })
      },
      sortBySmallPrice(){
        this.games = this.games.sort(function(a, b){
            a.price = a.price.replace(',','.')
            b.price = b.price.replace(',','.')
            return Number(a.price) == Number(b.price) ? 0 : +(Number(a.price) > Number(b.price)) || -1;
          })
      },
      handleToggleCart(){
        console.log('kkkkkk')
        this.cartMobileIsOpen = !this.cartMobileIsOpen
      },
      sortByRelevance(){
        this.games = games
      }
    },
    data(){
      return{
          options:[
            {title:"Mais populares",func:this.sortByRelevance},
            {title:"Menor preço",func:this.sortBySmallPrice},
            {title:"Maior preço",func:this.sortByBiggestPrice}
          ],
          defaultGames:[],
          games:games,
          selectedsItems:[],
          update:0,
          cartMobileIsOpen:false
      }
    },
    created(){
      console.log('render')
    },
    provide(){
      return {
        selectedsItems:this.selectedsItems,
        handleAddAmount:this.handleAddAmount,
        handleSubAmount:this.handleSubAmount,
        handleRemoveItem:this.handleRemoveItem,
      }
    },
    watch:{
      selectedsItems(old,newvalue){
        this.selectedsItems = newvalue
      }
    }
  }
</script>

<style>
  #topic{
    font-size: 3rem;
    font-weight: bold;
    font-family: Roboto, sans-serif;
  }
  #cart-mobile{
    top: 60px;
    z-index: 1;
    background-color: rgb(255, 255, 255);
  }
  #button-mobile-card{

  }
  #button-mobile-card span{
    font-size: 1.1rem;
  }
</style>
