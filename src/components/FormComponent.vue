<template>
     <form v-on:enter.prevent="addTodo">
    <input type="text" v-model="inputValue" />
    <button class="btn-reset" v-on:click.prevent="addTodo"></button>
    <!-- v-on:click => click 이벤트 리슨 (.prevent 속성을 추가하면 이벤트 전파를 중단함) -->
  </form>

</template>
 
<script>
export default {
    props:['value'], //부모로 부터 받아옴
    data () {
        return{
            inputValue : this.value
        }
    },
    methods:{
        addTodo () {
            console.log(this.inputValue)
            this.$emit('@submit',this.inputValue) //다시 부모로 전달 부모에서 받을때 on
           // this.inputValue=''
        }
    }
}
</script>

