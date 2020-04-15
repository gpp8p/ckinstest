<template>
    <span class="inputField">
        <span class="inputPrompt">{{configElement.prompt}}</span><span><input type="text" :size=configElement.fieldSize ref="titleText" v-focus="this.configElement.element==this.activeInputField"  @keydown.tab.exact = "textEntered" @keyup.enter="textEntered" v-model="value"/></span>
    </span>
</template>

<script>
  /* eslint-disable no-debugger,no-console */
  import { focus } from 'vue-focus';
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
      },
      activeInputField:{
        type: String,
        required: true
      }

    },
    data(){
      return{
        value: this.getCurrentValue(),
        focusActive: this.isActive
      }
    },
    directives: { focus: focus },
    watch:{
      currentValues: function(){
//          debugger;
        this.val = this.getCurrentValue();
      }
    },
    methods:{
      isActive(){
          debugger;
        if(this.configElement.element==this.activeInputField){
            return true;
        }else{
            return false;
        }
      },
      textEntered(){
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