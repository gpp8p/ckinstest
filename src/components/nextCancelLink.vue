<template>
    <span>
        <MyClickLink @myButtonClicked="nextClicked" v-if="this.currentStatus>0" buttonLabel="Next"></MyClickLink>
        <MyClickLink v-if="this.currentStatus>0"   @myButtonClicked="prevClicked" buttonLabel="Previous"></MyClickLink>
        <MyClickLink @myButtonClicked="cancelClicked" buttonLabel="Cancel"></MyClickLink>
    </span>
</template>

<script>
  import MyClickLink from "../components/MyClickLink.vue";
  export default {
    name: "nextCancelButtons",
    components: {MyClickLink},
    props: {
      currentStatus:{
        type: Number,
        required: true
      }
    },
    methods:{
      nextClicked(){
        this.$emit('buttonClick', ['next'])
      },
      prevClicked(){
        this.$emit('buttonClick', ['previous'])
      },
      cancelClicked(){
        this.$emit('buttonClick', ['cancel'])
      }
    }

  };
</script>

<style scoped>
    .errorCss {
        text-align: right;
        margin-right: 10px;
        color: red;
    }


</style>