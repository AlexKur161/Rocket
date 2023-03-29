<template>
  <div class="wrap-filter">
    <div class="wrap-filter_mini filter-attributes">
      <h3 class="wrap-filter_mini_title">Страна</h3>
      <div class="wrap-filter_mini_features wrap-filter-country">
        <button v-if="showClearBtn" class="clear_btn" @click="clearValue"><img src="../assets/img/close.svg" alt="close"></button>
        <input v-model="countriesFields" placeholder="Поиск стран" @change="serchcheckCountries" class="wrap-filter_input input_country" type="text">
      </div>
      <div class="wrap-filter_mini_features checkbox-features">
        <div v-if="showCountries">
        <CheckboxGroup
        :arrayCheckbox = "countriesSearch"
        @chekUpdateGroup="countriesCheck"
        />
        </div>
        <div v-if="!showCountries" class="checkbox-countries">
          <p class="checkbox-countries_title">К сожалению, по вашему запросу ничего не найдено :(</p>
        </div>
      </div>
      <h3 class="wrap-filter_mini_title">Тип</h3>
      <div class="wrap-filter_mini_features checkbox-features">
        <CheckboxGroup
        :arrayCheckbox = "type"
        @chekUpdateGroup="typeCheck"
        />
      </div>
      <h3 class="wrap-filter_mini_title">Количество звезд</h3>
      <div class="wrap-filter_mini_features checkbox-features">
        <CheckboxGroup
        :arrayCheckbox = "arr"
        @chekUpdateGroup="starCheck"
        />
      </div>
      <h3 class="wrap-filter_mini_title">Количество отзывов (от)</h3>
      <div class="wrap-filter_mini_features">
      <input v-model="reviews" class="wrap-filter_input" placeholder="Например от 10" type="number">
      </div>
      <h3 class="wrap-filter_mini_title">Цена</h3>
      <div class="wrap-filter_mini_price">
          <div class="wrap-filter_mini_features wrap-filter_mini_price_features">
            <p class="wrap-filter_input">{{barMinValue}}</p>
          </div>
          <img src="../assets/img/line.svg" alt="">
          <div class="wrap-filter_mini_features  wrap-filter_mini_price_features">
            <p class="wrap-filter_input">{{barMaxValue}}</p>
          </div>
      </div>
      <MultiRangeSlider
        :min="barMin"
        :max="barMax"
        :minValue="barMinValue"
        :maxValue="barMaxValue"
        :step="barStep"
        :ruler="barRuler"
        :label="barLabel"
        @input="UpdateValues"
      />
    <button class="wrap-filter_mini_btnfilter" @click="filterCards">Применить фильтр</button>
    <button class="wrap-filter_mini_btnfilter clearfilter" @click="clearFilter">Очистить фильтр</button>
    </div>
    <div class="wrap-filter_mini cards-wrap">
        <HotelCard
        :filterCard = "filterCard" />
    </div>
  </div>
  
</template>

<script>
import MultiRangeSlider from "multi-range-slider-vue";
import Checkbox from '../components/Checkbox.vue';
import CheckboxGroup from "../components/CheckboxGroup.vue";
import HotelCard from "../components/HotelCard.vue"
import {mapGetters, mapActions} from "vuex"

export default{
  data() {
    return {
      countriesFields: "",
      typeCheckbox: [],
      countriesCheckbox: [],
      starCheckbox: [],
      barMin: 0,
      barMax: 10000,
      barStep: 100,
      barMinValue: 0,
      barMaxValue: 10000,
      barRuler: false,
      barLabel: false,
      minPrice: 0,
      checkboxActive: true,
      info: [],
      filterCard: {},
      reviews: '',
      countriesSearch: [],
      countries: [
        {
          name: 'Албания',
          id: 3367,
          selected: false
        },
        {
          name: 'Азербайджан',
          id: 2252544,
          selected: false
        },
        {
          name: 'Греция',
          id: 243675760,
          selected: false
        },
        {
          name: 'Грузия',
          id: 253562372572,
          selected: false
        },
        {
          name: 'Германия',
          id: 32423487,
          selected: false
        }
      ],
      arr: [
        {
          name: '1 звезда',
          stars: 1,
          id: 132,
          selected: false
        },
        {
          name: '2 звезды',
          stars: 2,
          id: 2556684,
          selected: false
        },
        {
          name: '3 звезды',
          stars: 3,
          id: 37587,
          selected: false
        },
        {
          name: '4 звезды',
          stars: 4,
          id: 49878,
          selected: false
        },
        {
          name: '5 звезд',
          stars: 5,
          id: 597787,
          selected: false
        }
      ],
      type: [
        {
          name: 'Апартаменты',
          id: 4242478,
          selected: false
        },
        {
          name: 'Отель',
          id: 592534787,
          selected: false
        }
      ]
    }
  },
  components:{
      MultiRangeSlider,
      CheckboxGroup,
      Checkbox,
      HotelCard
  },
  computed: {
    ...mapGetters([
      "HOTELS",
      "FILTERHOTELS"
    ]),
    showCountries(){
      if(this.countriesSearch.length > 0){
        return true
      }else if (this.countriesFields === ""){
        return this.countriesSearch = this.countries
      }
    },
    showClearBtn(){
      if(this.countriesFields === ""){
        return false
      }else {
        return true
      }
    }
  },
  methods: {
    ...mapActions([
            'GET_HOTELS',
            'FILTER_CARD',
            'FILTER_CLEAR'
        ]),
        clearValue(){
          this.countriesFields = ""
          this.countries.map(hotelsItem => {
          hotelsItem.selected = false
          })
          this.countriesSearch = this.countries
        },
        serchcheckCountries(){
          if(this.countriesFields !== ""){
            this.countriesSearch = this.countries.filter(itemCountries => {
                return itemCountries.name.toLowerCase().includes(this.countriesFields.toLowerCase())
            })
          }else{
              this.countriesSearch = this.countries
            }
        },
        countriesCheck(params){
    let selectedCheckbox = this.countries.filter(item => {
        return item.id == params.id
      })
      let indexCheckbox = this.countries.indexOf(selectedCheckbox[0])
      this.countries[indexCheckbox].selected = params.selected
    },
    starCheck(params){
    let selectedCheckbox = this.arr.filter(item => {
        return item.id == params.id
      })
      let indexCheckbox = this.arr.indexOf(selectedCheckbox[0])
      this.arr[indexCheckbox].selected = params.selected
    },
    typeCheck(params){
      
    let selectedCheckbox = this.type.filter(item => {
        return item.id == params.id
      })
      let indexCheckbox = this.type.indexOf(selectedCheckbox[0])
      this.type[indexCheckbox].selected = params.selected
    },
      UpdateValues(e) {
        this.barMinValue = e.minValue;
        this.barMaxValue = e.maxValue;
      },
      maxInput(val){
        if(this.barMaxValue < 100){
          this.barMaxValue = 100
        }
      },
      minInput(e){
        if(e.target.value === ""){
          this.barMinValue = 0
        }
        if(this.barMinValue < 0  && this.barMinValue !== 0){
          this.barMinValue = 0
        }
      },
      filterCards(){
        this.typeCheckbox = []
        this.starCheckbox = []
        this.countriesCheckbox = []
              this.type.map(hotelsItem => {
              return this.HOTELS.map(item => {
                if((item.type === hotelsItem.name) && hotelsItem.selected){
                  return this.typeCheckbox.push(item.type)
                }
              })
            })
            this.arr.map(hotelsItem => {
                if(hotelsItem.selected){
                  return this.starCheckbox.push(hotelsItem.stars)
                }
            })
            this.countriesSearch.map(hotelsItem => {
                if(hotelsItem.selected){
                  return this.countriesCheckbox.push(hotelsItem.name.toLowerCase())
                }
            }) 

  this.filterCard = this.HOTELS
        // Фильтруем по стране
        .filter(product => {
            if(this.countriesCheckbox.length !== 0){
              return this.countriesCheckbox.includes(product.country.toLowerCase())
            }else{
              return true
            }
          }) 
        // Фильтруем по типу
        .filter(product => {
            if(this.typeCheckbox.length !== 0){
              return this.typeCheckbox.includes(product.type)
              
            }else{
              return true
            }
          })
        // Фильтруем по звездам
        .filter(product => {
            if(this.starCheckbox.length !== 0){
              return this.starCheckbox.includes(product.stars)
            }else{
              return true
            }
          })
           // Фильтруем по количеству отзывов
        .filter(product => {
            if(this.reviews !== ''){
              return product.reviews_amount >= this.reviews
            }else{
              return true
            }
          })
          // Фильтруем по цене
        .filter(product => {
          return product.min_price >= this.barMinValue && product.min_price <= this.barMaxValue
        })

this.FILTER_CARD(this.filterCard)
      },
      clearFilter(){
        this.FILTER_CLEAR()
        this.barMinValue = 0
        this.barMaxValue = 10000
        this.type.map(hotelsItem => {
          hotelsItem.selected = false
        })
        this.arr.map(hotelsItem => {
          hotelsItem.selected = false
        })
        this.countries.map(hotelsItem => {
          hotelsItem.selected = false
        })
        this.countriesSearch = this.countries
        this.countriesFields = ""
        this.reviews = ""
      }
  }
}

</script>

<style lang="scss" scoped>
.wrap-filter{
  display: flex;
  justify-content: space-between;
  padding-top: 56px;
  gap: 40px;
  &_input{
    width: 100%;
    height: 100%;
    padding: 15px 20px;
    font-family: 'PT Sans';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 21px;
    color: #868686;
    border: none;
    outline: none;
  }
  .filter-attributes{
    max-width: 325px;
  }
  .cards-wrap{
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  &_mini{
    width: 100%;
    &_btnfilter{
      background: #6A53F5;
      border-radius: 15px;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 17px 95px;
      font-family: 'PT Sans';
      font-style: normal;
      font-weight: 700;
      font-size: 16px;
      line-height: 21px;
      color: #FFFFFF;
      border: none;
      margin-top: 30px;
      cursor: pointer;
      width: 100%;
    }
    .clearfilter{
      background: none;
      margin-top: 15px;
      color: #3A3A3A;
      border: 1px solid #EAEAEA;
    }
    &_title{
      font-size: 16px;
      line-height: 21px;
      color: #3A3A3A;
      margin-bottom: 14px;
    }
    &_features{
      border: 1px solid #EAEAEA;
      border-radius: 10px;
      width: 325px;
      margin-bottom: 25px;
      .input_country{
        padding-left: 46px;
        position: relative;
      }
      .checkbox-countries{
        display: flex;
        justify-content: center;
        align-items: center;
        &_title{
          font-weight: 400;
          font-size: 16px;
          line-height: 24px;
          text-align: center;
          color: #868686;
        }
      }
    }
    .checkbox-features{
      padding: 26px 20px;
    }
    .wrap-filter-country{
      position: relative;
      .clear_btn{
        position: absolute;
        right: 20px;
        top: 50%;
        transform: translate(0, -50%);
        width: 13px;
        height: 13px;
        z-index: 3;
        border: none;
        background: none;
        cursor: pointer;
      }
    }
    .wrap-filter-country::before{
        content:"";
        position: absolute;
        width: 17px;
        height: 17px;
        background-image: url("../assets/img/search.svg");
        background-size: cover;
        background-repeat: no-repeat;
        top: 50%;
        transform: translate(0, -50%);
        left: 20px;
        z-index: 3;
      }
    &_price{
      display: flex;
      justify-content: space-between;
      align-items: center;
      &_features{
        width: auto;
        margin-bottom: 0;
        width: 148px;
      }
    }
  }
}
</style>
