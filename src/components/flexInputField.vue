<template>
    <span class="inputField">
        <span class="inputPrompt">{{configElement.prompt}}</span><span><input type="text" :size=configElement.fieldSize ref="titleText"  @keyup.enter="titleEntered" v-model="value"/></span>
    </span>
</template>

<script>
  /* eslint-disable no-debugger,no-console */

  export default {
    name: "flexInputField",
    props: {
      configElement: {
        type: Object,
        required: true
      },
      currentValues:{
        type: Object,
        required: false
      }

    },
    data(){
      return{
        value: this.getCurrentValue()
      }
    },
    watch:{
      currentValues: function(){
//          debugger;
        this.val = this.getCurrentValue();
      }
    },
    methods:{
      titleEntered(){
//        debugger;
        this.currentValues[this.configElement.element]=this.value;
        this.$emit('configSelected', [this.configElement.element, this.value, null, null,true]);
      },
      getCurrentValue(){
//        debugger;
        if(typeof(this.currentValues[this.configElement.element])=='undefined'&& this.value=='') {
          return '';
        }else{
          var colonDelimiterLocatedAt= this.currentValues[this.configElement.element].indexOf(":");
          var thisProp = this.currentValues[this.configElement.element].substr(colonDelimiterLocatedAt+1);
          return thisProp;
        }
      }
// eslint-disable-next-line no-debugger
    },
  };
</script>

<style scoped>
    .inputField {
        display: grid;
        grid-template-columns: 30% auto;
    }
    .inputPrompt {
        font-family: Geneva;
        font-size: 12px;
        font-style: normal;
        font-weight: bold;
    }
</style>