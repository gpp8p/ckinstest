<template>
    <span><input type="checkbox" @click="this.clicked" :checked="checked" value="on" /></span>
</template>

<script>
    export default {
        name: "permListPerm",
        props:{
            permType:{
                type: String,
                required: true
            },
            permValue:{
                type: Number,
                required:true
            }
        },
        data(){
          return{
              checked: false
          }
        },
        methods:{
            clicked(event){
                if (event.target.checked){
                    this.checked=true;
                    this.$emit('permChanged', [this.permType,'activated']);
                }else{
                    this.checked=false;
                    this.$emit('permChanged', [this.permType,'deactivated']);
                }
            },
        }
    }
</script>

<style scoped>

</style>
