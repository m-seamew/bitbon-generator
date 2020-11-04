<template>
  <div class="itemlist itemlist--white">
    <div class="itemlist__leftside">
      <div class="itemlist__img">
        <img :src="item.logo" :alt="item.name" class="itemlist__logo">
      </div>
      <div class="itemlist__leftside--title">
        <div class="itemlist__title">
          {{item.name}}
        </div>
        <div class="itemlist__ticker">
          {{item.ticker}}
        </div>
      </div> 
    </div>
    <div class="itemlist__rightside">
      <div class="itemlist__rightside--title" :class="styled">
        <div class="itemlist__curprice">
          {{parseFloat(item.curPrice).toLocaleString('en-IE')}} 
        </div>
        <div class="itemlist__change">
          <div v-if="!item.isMarketOpen">рынок закрыт</div>
          <div v-else-if="item.change > 0">{{'+'+item.change}}%</div>
          <div v-else>{{item.change}}%</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    item:{
      type: Object,
      required: true,
    }
  },
  computed:{
    styled(){
      if(this.item.isMarketOpen === true){
        if(this.item.change > 0){
          return 'itemlist__rightside--grow';
        } else if (this.item.change < 0){
          return 'itemlist__rightside--down';
        }

      }
    }
  }
}
</script>

<style lang="scss">
  .itemlist--white{
    color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: space-between;     
  }

  .itemlist__leftside{
    display: flex;
    align-items: center;
  }

  .itemlist__leftside--title{
    text-align: left;
  }
  .itemlist__rightside--title{
    text-align: right;
  }

  .itemlist__rightside--title{
    padding: 15px 30px;
    border-radius: 13px;
    background-color: #dec549;
  }

  .itemlist__rightside--grow{
    background-color: #22d88e;
  }

  .itemlist__rightside--down{
    background-color: #f97070;
  }

  .itemlist__logo{
    width:100%;
    height: 100%;
    object-fit: contain;
  }

  .itemlist__ticker{
        margin-left: 1px;
  }
</style>