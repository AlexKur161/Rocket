<template>
        <div :class="{ checkform: cheked }" class="form-group">
            <input :value="title" type="checkbox" :id="id">
            <label class="title-checkbox" :for="id">{{title}}</label>
        </div>
</template>
<script>
export default {
    props:{
        title:{
            type: String,
            required: true
        },
        cheked:{
            type: Boolean,
            required: true
        },
        id:{
            type: Number,
            required: true
        }
    },
    data(){
        return{
            chek: false
        }
    },
    computed:{
        checkInput(){
            if(this.cheked === false){
                return this.chek = false
            }else{
                return this.chek = true
            }
        }
    }
}
</script>
<style lang="scss" scoped>
.form-group input {
  padding: 0;
  height: initial;
  width: initial;
  margin-bottom: 0;
  display: none;
  cursor: pointer;
}

.form-group label {
  position: relative;
  cursor: pointer;
}

.form-group label:before {
    content:'';
    -webkit-appearance: none;
    background-color: transparent;
    border: 1px solid #CDCDCD;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
    padding: 10px;
    display: inline-block;
    position: relative;
    vertical-align: middle;
    cursor: pointer;
    margin-right: 5px;
    border-radius: 4px;
}
.checkform label:before {
    background: #00BB6D;
    border: 1px solid #00BB6D;
}

.form-group input:checked + label:after {
    content: '';
    display: block;
    position: absolute;
    top: 2px;
    left: 9px;
    width: 3px;
    height: 9px;
    border: solid #fff;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
}
.title-checkbox{
    font-family: 'PT Sans';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 21px;
    color: #3A3A3A;
}
</style>