<template>
  <div>
    <img src="@/assets/IMG_9012.png" alt="money" class="insta__img insta__img--top">
    <img src="@/assets/IMG_90122.png" alt="money" class="insta__img insta__img--bottom">
    <img src="@/assets/IMG_9013.png" alt="money" class="insta__img insta__img--center">
    <img src="@/assets/IMG_9099.png" alt="money" class="insta__img insta__img--left">

    <div class="insta__heder">
      <header-title/>
    </div>
    <div class="insta__container">
      <div v-for="(element, index) in itemList" class="insta__item" :key="index">
        <item-list :item="element"/>
      </div>
      <div class="insta__item insta__footer">
        <div class="insta__bitbontoday-logo">
          <img src="@/assets/logo.png" alt="money" class="bitbontoday-logo">
        </div>
        <div class="footer__text">
          <span class="footer__text--bold">Еще больше информации в нашем Telegram канале и напортале Bitbon.Today.</span> Ссылка в шапке профиля, переходите и делитесь с друзьями.
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