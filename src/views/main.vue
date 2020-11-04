<template>
  <section class="screen">
    <div class="navigation">
      <button class="button button--generate"  @click="generateImg">Cгенерировать</button>
      <button class="button button--download" @click="getImg">Cкачать</button>
    </div>

    <div class="screen--main">
      <div class="preloader preloader--insta">
        <div class="insta">
          <insta-img :itemList="itemList"></insta-img>    
        </div>
      </div>

      <div class="fullsize">
        <div class="insta" id="insta">
          <insta-img :itemList="itemList"></insta-img>    
        </div>
      </div>
      
      <div class="second">
        <div class="preloader preloader--facebook">
          <div class="facebook">
            <facebook-img :itemList="itemList"></facebook-img>    
          </div>
        </div>

        <div class="preloader preloader--twitter">
          <div class="twitter">
            <twitter-img :itemList="itemList"/>    
          </div>
        </div>
      </div>

       <div class="fullsize">
        <div class="facebook" id="facebook">
          <facebook-img :itemList="itemList"></facebook-img>    
        </div>
      </div>

      <div class="fullsize"> 
        <div class="twitter" id="twitter">
          <twitter-img :itemList="itemList"></twitter-img>    
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import insta from '@/components/insta'
import facebook from '@/components/facebook'
import twitter from '@/components/twitter'

import axios from 'axios';
import html2canvas from 'html2canvas';

import firebase from "firebase/app";
require('firebase/auth');
require('firebase/database');


//https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=ethereum%2Cbitcoin&order=market_cap_desc&per_page=100&page=1&sparkline=false&price_change_percentage=24h  
//https://financialmodelingprep.com/api/v3/quotes/index?apikey=d135631d70679604e7b02ace378dd43a
//https://data-asg.goldprice.org/dbXRates/USD
export default {
  components:{
    'insta-img': insta,
    'facebook-img': facebook,
    'twitter-img': twitter,
  },
  data: function(){
    return{
      itemList: [
        { 
          id: 'bitcoin',
          name: 'Bitcoin',
          ticker: 'BTC',
          curPrice: 0,
          change: 0,
          isMarketOpen: true,
          logo: require('@/assets/bitcoin.png')
        },
        {
          id: 'ethereum',
          name: 'Ethereum',
          ticker: 'ETH',
          curPrice: 0,
          change: 0,
          isMarketOpen: true,
          logo: require('@/assets/ethereum.png')
        },
        {
          id: 'bitbon',
          name: 'Bitbon',
          ticker: 'BITBON',
          curPrice: 0,
          change: 0,
          isMarketOpen: true,
          logo: require('@/assets/bitbon.png')
        },
        {
          id: 'gold',
          name: 'Золото',
          ticker: 'Aurum',
          curPrice: 0,
          change: 0,
          isMarketOpen: true,
          logo: require('@/assets/gold.png')
        },
        {
          id: 'sp500',
          name: 'S&P 500',
          ticker: 'S&P 500',
          curPrice: 0,
          change: 0,
          isMarketOpen: true,
          logo: require('@/assets/s&p500.png')
        },
      ],
      imgArr: [
          {
            id: 'insta',
            width: 1242,
            height: 2691,
            name: 'insta.jpg',
            scale: 1,
          },
          {
            id: 'facebook',
            width: 1909,
            height: 1835,
            name: 'facebook.jpg',
            scale: 1,
          },
          {
            id: 'twitter',
            width: 800,
            height: 512,
            name: 'twitter.jpg',
            scale: 2,
          },
        ]
    }
  },
  methods:{
    generateImg(){
     axios
      .get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=ethereum%2Cbitcoin&order=market_cap_desc&per_page=100&page=1&sparkline=false&price_change_percentage=24h')
      .then(res => {
        let data = res.data;
        data.forEach(el => {
          this.itemList.forEach(element=> {
            if(element.id == el.id){
              element.curPrice = el.current_price;
              element.change = el.market_cap_change_percentage_24h.toFixed(2);
              return;
            }
          });
        });
      })
      .then(_=>{
        axios
          .get('https://data-asg.goldprice.org/dbXRates/USD')
          .then(response => {
            let data = response.data.items[0];

            this.itemList.forEach(element=> {
              if(element.id == 'gold'){
                element.curPrice = data.xauPrice.toFixed(2);
                element.change = data.pcXau.toFixed(2);

                let tempDate = new Date()
                let dayOfWeek = new Date(tempDate).getUTCDay();
                let timeOfDay = new Date(tempDate).getUTCHours();
                
                switch(dayOfWeek){
                  case 5: 
                    timeOfDay >= 20 ? element.isMarketOpen = false : null;
                    break;
                  case 6:
                    element.isMarketOpen = false;
                    break;
                  case 0: 
                    timeOfDay < 23 ? element.isMarketOpen = false : null;
                    break;
                }

                return;
              }
            });
          })
      })
      .then(_=>{
        axios
          .get('https://bc-api.bit.trade/api/price?currency=usd')
          .then(response => {
            this.itemList.forEach(element=> {
              if(element.id == 'bitbon'){
                element.curPrice = response.data;
                let currentPrice = element.curPrice;

                const database = firebase.database();
                const bitbonHistoryPrice = database.ref("/bitbonCurList");
                bitbonHistoryPrice.once("value")
                  .then(snapshot => {
                    let tempSnap = snapshot.val();
                    let tempDate = new Date().getTime();
                    let checkedPeriod = tempDate - 86400000;
                    let tempArr = [];

                    if (tempSnap !== 0) {
                      for (let i = 0; i < Object.keys(tempSnap).length; i++) {
                        tempArr.push({time: tempSnap[Object.keys(tempSnap)[i]].date, price: tempSnap[Object.keys(tempSnap)[i]].price});
                      }
                      tempArr = tempArr.reverse();
              
                      let checkedListItem = tempArr.find(el=> el.time <= checkedPeriod);
                      let priceForCalculation = 0;
                    
                      checkedListItem !== undefined ? priceForCalculation = checkedListItem.price : priceForCalculation = tempArr[tempArr.length-1].price;
              
                      let result = 0;
              
                      switch(true){
                        case currentPrice > priceForCalculation:
                          result = ((currentPrice - priceForCalculation) / priceForCalculation * 100).toFixed(2);
                          break;
                        case currentPrice < priceForCalculation: 
                          result = 0 - ((priceForCalculation - currentPrice) / priceForCalculation * 100).toFixed(2);
                          break;
                        case currentPrice == priceForCalculation:
                          result = 0;
                          break;    
                      }

                    element.change = result;
                    console.log('результат -' + result);
                    }  
                  });
              }
           });
          })
      }) 
      .then(_=>{
        axios
        .get('https://financialmodelingprep.com/api/v3/quotes/index?apikey=d135631d70679604e7b02ace378dd43a')
          .then(res => {
            let data = res.data;
            let checked = data.find(el=> el.symbol == '^GSPC');
          
            if(checked !== undefined){
              this.itemList.forEach(element=> {
                if(element.id == 'sp500'){
                  element.curPrice = checked.price.toFixed(2);
                  element.change = checked.changesPercentage.toFixed(2);

                let tempDate = new Date()
                let dayOfWeek = new Date(tempDate).getUTCDay();
                let timeOfDay = new Date(tempDate).getUTCHours();
                
                switch(true){
                  case dayOfWeek > 0 && dayOfWeek < 6: 
                    timeOfDay >= 21 || timeOfDay <= 1 ? element.isMarketOpen = false : null;
                    break;
                  case dayOfWeek == 6 || dayOfWeek == 0:
                    element.isMarketOpen = false;
                    break;
                }

                return
                }
               })
            }
          })
      })
    },
    getImg(){
      this.imgArr.forEach(element =>{
      const checked = document.getElementById(element.id);
      //checked.style.display="block";
      html2canvas(checked,
      {
        width: element.width,
        height: element.height,
        scale:element.scale, 
      })
        .then(canvas => {
          //document.getElementById('insta').style.display="none";
          const a = document.createElement('a');
            a.href = canvas.toDataURL("image/jpeg", 1);       
            a.download = element.name;
            a.click();
        })
      })
    },
    
  },
  created(){
    const firebaseConfig = {
    apiKey: "AIzaSyBalplxH5kHQF7ItaykD8P_2DCyVyOMyaw",
    authDomain: "bitbon-d4747.firebaseapp.com",
    databaseURL: "https://bitbon-d4747.firebaseio.com",
    projectId: "bitbon-d4747",
    storageBucket: "bitbon-d4747.appspot.com",
    messagingSenderId: "640509602580",
    appId: "1:640509602580:web:b6e266cfa072cb5e8e3001"
    };

    if (!firebase.apps.length) {
      firebase.initializeApp(firebaseConfig);
      firebase.auth().setPersistence(firebase.auth.Auth.Persistence.SESSION)
        .then( _=>{
          firebase.auth().signInWithEmailAndPassword('bitbonprices@bitbon.today', 'bitbonprices')
            .catch(error =>console.log(errorCode, 'error messages:'+" "+error.message));
        });
    }
  }
}
</script>

<style lang="scss">

.button{
  border: none;
  padding: 15px 25px;
  outline: none;
  margin: 10px;
  border-radius: 10px;
  background-image: linear-gradient(90deg,#565dd6,#8147c1);
  color: #fff;
  border-radius: 10px;
  font-size: 18px;
  cursor: pointer;
  font-weight: 600;
}

.button--download{
  border: 2px solid #7957d5;
  color: #7957d5;
  background-image: none;
  background-color: white;
}

.navigation{
  display: flex;
  margin-left: 40px;
}

.test-tw{
  position: relative;
}
.test-cont{
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 99;
  opacity: 0.4;
}

.test-img{
  width: 100%;
  object-fit: contain;
}
.screen--main{
  display: flex;
}

.preloader{
  padding: 20px;
}

.preloader--insta{
  max-width: calc(1242px*0.3);
  max-height: calc(2691px*0.3);
}

.preloader .insta{
  transform: translate(-35%, -35%) scale(0.3);
}

.preloader--facebook{
  max-width: calc(1911px*0.3);
  max-height: calc(1835px*0.3);
}

.preloader--twitter{
  max-width: calc(800px*0.5);
  max-height: calc(512px*0.5);
}
.preloader .twitter{
  transform: translate(-25%, -25%) scale(0.5);
}

.preloader .facebook{
  transform: translate(-35%, -35%) scale(0.3);
}


.facebook{
  width: 1909px;
  height: 1835px;
  background-image: linear-gradient(173deg, #1084ba 0%, #11c2d6 100%);
  position: relative;
 // display: none;
}



.bitbontoday-logo{
  width: 100%;
  object-fit: contain;
}

.footer__text{
  color: #ffffff;
  text-align: left;
}
.footer__text--bold{
  font-weight: 700;
}

.fullsize{
  transform: translateX(-110%);
  position: absolute;
  z-index: 10;
}
</style>