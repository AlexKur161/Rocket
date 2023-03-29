<template>
<div class="group-checkbox">
    <Checkbox
    :cheked="checkbox.selected"
    :title="checkbox.name"
    :id="checkbox.id"
    v-for="checkbox in arrayCheckbox"
    :key="checkbox.id"
    @input="chekUpdateGroup($event)"
    />
    </div>
</template>
<script>
import Checkbox from "../components/Checkbox.vue"
export default {
    props:{
        arrayCheckbox:{
            type: Array,
            required: true
        }
    },
    data(){
        return{
            testCheck: []
        }
    },
    methods:{
        chekUpdateGroup(e){
            this.$emit('chekUpdateGroup', {
                id: e.target.id,
                selected: e.target.checked
            })
        }
    },
    components:{
        Checkbox
    }
}
</script>
<style lang="scss" scoped>
.group-checkbox{
    display: flex;
    flex-direction: column;
    gap: 15px;
}
</style>