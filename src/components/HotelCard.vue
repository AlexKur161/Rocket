<template>
<div class="card-wrap" v-if="FILTERHOTELS === 0">
    <div v-for="(hotel, i) in displayedPosts" :key="i" class="card">
        <div class="card_description">
            <h4 class="card_description_title">{{hotel.name}}</h4>
            <div class="card_description_attributes">
                <div class="card_description_attributes_mini">
                    <div class="star-wrap">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                    </div>
                </div>
                <div class="card_description_attributes_mini">
                    <h5 class="type_housing">{{hotel.type}}</h5>
                </div>
                <div class="card_description_attributes_mini">
                    <img src="../assets/img/elipse.svg" alt="elips">
                    <h5 class="reviews">{{hotel.reviews_amount}} Отзыва</h5>
                </div>
                <div class="card_description_attributes_mini">
                    <img src="../assets/img/indicator.svg" alt="indicator">
                    <h5 class="country">{{hotel.country}}</h5>
                </div>
            </div>
            <p class="specification">{{hotel.description}}</p>
        </div>
        <div class="card_price">
            <p class="card_price_title">{{hotel.min_price}} ₽</p>
            <p class="card_price_night">Цена за 1 ночь</p>
            <button class="card_price_btn">
                <img src="../assets/img/bron.svg" alt="bron">
                <p>Забронировать</p>
            </button>
        </div>
    </div>

    <div  class="pagination-wrap">
    <button type="button" class="btn-pagination" :disabled="page === 1" @click="page--"> Назад </button>
    <button type="button" class="btn-pagination" :disabled="page > (HOTELS.length / 3)" @click="page++"> Следующая</button>
	</div>

    </div>
    <div class="card-wrap" v-if="FILTERHOTELS.length !== 0 && FILTERHOTELS !== 0">
    <div v-for="(hotel, i) in displayedPostsFilter" :key="i" class="card">
        <div class="card_description">
            <h4 class="card_description_title">{{hotel.name}}</h4>
            <div class="card_description_attributes">
                <div class="card_description_attributes_mini">
                    <div class="star-wrap">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                        <img src="../assets/img/star.svg" alt="star">
                    </div>
                </div>
                <div class="card_description_attributes_mini">
                    <h5 class="type_housing">{{hotel.type}}</h5>
                </div>
                <div class="card_description_attributes_mini">
                    <img src="../assets/img/elipse.svg" alt="elips">
                    <h5 class="reviews">{{hotel.reviews_amount}} Отзыва</h5>
                </div>
                <div class="card_description_attributes_mini">
                    <img src="../assets/img/indicator.svg" alt="indicator">
                    <h5 class="country">{{hotel.country}}</h5>
                </div>
            </div>
            <p class="specification">{{hotel.description}}</p>
        </div>
        <div class="card_price">
            <p class="card_price_title">{{hotel.min_price}} ₽</p>
            <p class="card_price_night">Цена за 1 ночь</p>
            <button class="card_price_btn">
                <img src="../assets/img/bron.svg" alt="bron">
                <p>Забронировать</p>
            </button>
        </div>
    </div>
    <div class="pagination-wrap">
    <button type="button" class="btn-pagination" :disabled="filterpage === 1" @click="filterpage--"> Назад </button>
    <button type="button" class="btn-pagination" @click="filterpage++" :disabled="filterpage > (FILTERHOTELS.length / 3)"> Следующая</button>
    </div>
    </div>
    
    <div class="card-no_find" v-if="FILTERHOTELS.length === 0 && FILTERHOTELS !== 0">
        <p class="no_find">записей не найдено</p>
    </div>
</template>

<script>
import {mapActions, mapGetters} from "vuex"

export default {
    props: {
        filterCard:{
            type: Object,
            required: true
        }
    },
    data(){
        return {
            page: 1,
            perPage: 3,
            pages: [],
            filterpage: 1,
            filterperPage: 3,
            filterpages: [],
        }
    },
    computed:{
        ...mapGetters([
            'HOTELS',
            'FILTERHOTELS'
        ]),
        displayedPosts () {
	      return this.paginate(this.HOTELS);
	    },
        displayedPostsFilter () {
	      return this.paginateFilter (this.FILTERHOTELS);
	    }
    },
    methods: {
        ...mapActions([
            'GET_HOTELS'
        ]),
        setPages () {
	      let numberOfPages = Math.ceil(this.HOTELS.length / this.perPage);
	      for (let index = 1; index <= numberOfPages; index++) {
	        this.pages.push(index);
	      }
	    },
        setPagesFilter () {
	      let numberOfPages = Math.ceil(this.FILTERHOTELS.length / this.filterperPage);
	      for (let index = 1; index <= numberOfPages; index++) {
	        this.filterpages.push(index);
	      }
	    },
	    paginate (posts) {
	      let page = this.page;
	      let perPage = this.perPage;
	      let from = (page * perPage) - perPage;
	      let to = (page * perPage);
	      return  posts.slice(from, to);
	    },
        paginateFilter (posts) {
	      let page = this.filterpage;
	      let perPage = this.filterperPage;
	      let from = (page * perPage) - perPage;
	      let to = (page * perPage);
	      return  posts.slice(from, to);
	    }
    },
    mounted(){
        this.GET_HOTELS()
    },
    watch: {
	    posts () {
	      this.setPages();
	    },
        postsfilter () {
	      this.setPagesFilter();
	    }
	  },
}
</script>
<style lang="scss" scoped>
.card-wrap{
    display: flex;
    flex-direction: column;
    gap: 15px;
    .pagination-wrap{
        display: flex;
        justify-content: center;
        gap: 40px;
        .btn-pagination{
            font-weight: 700;
            font-size: 16px;
            line-height: 24px;
            color: #3A3A3A;
            cursor: pointer;
            background: none;
            border: none;
        }
        .btn-pagination:disabled{
            color: #DBDBDB;
        }
    }
}
.card-no_find{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100%;
        .no_find{
            font-size: 24px;
            margin: auto;
    }
}
.card{
    border: 1px solid #EAEAEA;
    border-radius: 15px;
    padding: 25px 25px 28px 25px;
    width: 100%;
    display: flex;
    justify-content: space-between;
    &_price{
        display: flex;
        flex-direction: column;
        align-items: flex-end;
        &_title{
            font-weight: 700;
            font-size: 25px;
            line-height: 32px;
            text-align: right;
            color: #3A3A3A;
        }
        &_night{
            font-weight: 400;
            font-size: 12px;
            line-height: 16px;
            color: #868686;
            margin-bottom: 23px;
        }
        &_btn{
            display: flex;
            justify-content: center;
            gap: 10px;
            align-items: center;
            font-weight: 400;
            font-size: 14px;
            line-height: 18px;
            color: #6A53F5;
            background: #F1EEFE;
            border-radius: 12px;
            border: none;
            padding: 13px 30px 15px 30px;
            cursor: pointer;
        }
    }
    &_description_title{
        font-weight: 700;
        font-size: 20px;
        line-height: 26px;
        color: #3A3A3A;
        margin-bottom: 8px;
    }
    &_description_attributes{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 18px;
        max-width: 360px;
        &_mini{
            display: flex;
            gap: 8px;
            .star-wrap{
                display: flex;
                gap: 6px;
                margin-right: 15px;
            }
            .type_housing{
                font-weight: 400;
                font-size: 14px;
                line-height: 18px;
                color: #868686;
                margin-right: 6px;
            }
            .reviews{
                font-weight: 400;
                font-size: 14px;
                line-height: 18px;
                color: #868686;
                margin-right: 15px;              
            }
            .country{
                font-size: 14px;
                line-height: 21px;
                font-weight: 400;
                color: #3A3A3A;
                margin-top: 2px;
            }
        }
    }
}
</style>