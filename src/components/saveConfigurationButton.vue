<template>
    <span class="saveConfigButton">
        <MyButton @myButtonClicked="saveConfiguration" buttonLabel="Save Configuration"></MyButton>
    </span>

</template>

<script>
  /* eslint-disable no-debugger */

  import MyButton from "../components/MyButton.vue";
  export default {
    name: "saveConfigurationButton",
    components:{MyButton},
    methods:{
      saveConfiguration(){
//        debugger;
        this.$emit('configSelected', ['saveConfiguration','activated']);
      }
    },
    props: {
      configElement: {
        type: Object,
        required: true
      }
    },
    currentValues:{
      type: Object,
      required: false
    }

  };
</script>

<style scoped>
    .saveConfigButton {
        margin-left: 35%;
    }

</style>