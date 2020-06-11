<template>
  <div id="app" class="row justify-content-center m-0">
    <div class="col-11">
      <div class="row pt-5">
        <div class="col-9">
          <div class="row d-flex justify-content-between pb-4">
            <h1 id="topic">Games</h1>
            <div class="col-7 col-sm-3 d-flex align-items-center">
              <Select 
                :options="options" 
                :sortByBiggestPrice="sortByBiggestPrice"
                :sortBySmallPrice="sortBySmallPrice"  
              />
            </div>
          </div>
          <div class="row justify-content-between">
              <div class="col-6 col-sm-6 col-md-4 pl-0 pb-5"  v-for="(game,i) in games" :key="i">
                <Card :data="game" v-bind:onClickAdd="onClickAdd"/>
              </div>
          </div>
        </div>
        <div class="col-3">
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
      sortByRelevance(){
        this.games = this.defaultGames
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
</style>
