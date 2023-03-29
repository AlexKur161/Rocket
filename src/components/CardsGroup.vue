<template>
    <HotelCard
    v />
</template>
<script>
import HotelCard from "../components/HotelCard.vue"
import {mapActions, mapGetters} from "vuex"

export default {
    components:{
        HotelCard
    },
    computed:{
        ...mapGetters([
            'HOTELS'
        ])
    },
    methods: {
        ...mapActions([
            'GET_HOTELS'
        ])
    },
    mounted(){
        this.GET_HOTELS()
    }
}
</script>
<style lang="scss" scoped>

</style>