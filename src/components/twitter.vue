<template>
  <div>
    <div class="twitter__img-container--top twitter__img--top">
      <img src="@/assets/IMG_9012.png" alt="money" class="twitter__img">
    </div>

    <div class="twitter__img-container--bottom twitter__img--bottom">
      <img src="@/assets/IMG_90122.png" alt="money" class="twitter__img">
    </div>
    
    <div class="twitter__img-container--right-bottom twitter__img--right-bottom">
      <img src="@/assets/IMG_90122.png" alt="money" class="twitter__img">
    </div>

    <div class="twitter__heder">
      <header-title/>
    </div>
    <div class="twitter__container">
      <div v-for="(element, index) in itemList" class="twitter__item" :class='`itemist__`+element.id' :key="index">
        <item-list :item="element"/>
      </div>
      <div class="twitter__item twitter__footer">
        <div class="twitter__bitbontoday-logo">
          <img src="@/assets/logo.png" alt="money" class="bitbontoday-logo">
        </div>
        <div class="footer__text">
          <span class="footer__text--bold">Еще больше информации в нашем Telegram канале и напортале Bitbon.Today.</span> <br>Ссылка в шапке профиля, переходите и делитесь с друзьями.
        </div>
      </div>
    </div> 
  </div>
</template>

<script>
import HeaderTitle from '@/components/header';
import ItemList from '@/components/itemlist';

export default {
  props:{
    itemList:{
      type: Array,
      required: true,
    }
  },
  components:{
    'header-title': HeaderTitle, 
    'item-list': ItemList,
  },
}
</script>