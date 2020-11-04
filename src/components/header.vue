<template>
  <div class="header">
    <div class="header__leftside">
      <div class="header__lefttside-text">Курс к USD</div>
      <div class="header__img-box">
        <img src="@/assets/IMG_9011.png" alt="money" class="header__lefttside-img">
      </div>   
    </div>
    <div class="header__rightside">
      <p class="header__rightside-text">±% - отклонение от максимума за последний 24 часовой период.</p>
    </div>
  </div>
</template>

<style lang="scss">
 .header{
   display: flex;
   align-items: center;
 }
 .header__leftside{
   display: flex;
   flex-shrink: 0;
   align-items: center;
   padding-top: 25px;
   padding-bottom: 25px; 
   background-color: white;
   border-top-right-radius: 25px;
   border-bottom-right-radius: 25px;
 }
 .header__lefttside-text{
   margin-left: 70px;
   margin-right: 66px;
 }
 .header__rightside-text{
   margin-left: 30px;
   text-align: left;
   letter-spacing: 0.5px;
   padding-right: 50px; 
 }
 .header__lefttside-img{
   margin-right: 45px;
 }
</style>