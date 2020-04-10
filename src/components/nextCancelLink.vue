<template>
    <span>
        <MyClickLink v-if=showNext @myButtonClicked="nextClicked"  buttonLabel="Next"></MyClickLink>
        <MyClickLink v-if=showPrev @myButtonClicked="prevClicked" buttonLabel="Previous"></MyClickLink>
        <MyClickLink v-if=showSave @myButtonClicked="saveClicked" buttonLabel="Save"></MyClickLink>
        <MyClickLink @myButtonClicked="cancelClicked" buttonLabel="Cancel"></MyClickLink>
    </span>
</template>

<script>
  import MyClickLink from "../components/MyClickLink.vue";
  export default {
    name: "nextCancelButtons",
    components: {MyClickLink},
    props: {
      showNext:{
        type: Boolean,
        required: true
      },
      showPrev:{
          type: Boolean,
          required: true
      },
      showSave:{
          type: Boolean,
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
      },
      saveClicked(){
         this.$emit('buttonClick', ['save'])
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