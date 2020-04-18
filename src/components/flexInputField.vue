<template>
    <span class="inputField">
        <span class="inputPrompt">{{configElement.prompt}}</span><span><input ref="inputField" type="text" :size=configElement.fieldSize  @keydown.tab.exact = "tabErr" @keyup.enter="textEntered" v-model="value"/></span>
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
      },
      activeInputField:{
        type: String,
        required: true
      }

    },
    data(){
      return{
        value: this.getCurrentValue(),
        focusActive: this.isActive,
        filterComponents:[]
      }
    },
    watch:{
      activeInputField: function(){
//        debugger;
        console.log('activeInputField is'+ this.activeInputField+" element is:"+this.configElement.element);
        if(this.isActive()){
            console.log('was active');
            this.$refs.inputField.focus();
        }
      },
      currentValues: function(){
//          debugger;
        this.val = this.getCurrentValue();
          if(this.isActive()){
              this.$refs.inputField.focus();
          }
      }
    },
    mounted(){
//        debugger;
        if(this.isActive()){
            console.log(this.configElement.element+' is active');
            this.$refs.inputField.focus();
        }
    },
    methods:{
      isActive(){
//          debugger;
        if(this.configElement.element==this.activeInputField){
            return true;
        }else{
            return false;
        }
      },
      tabErr(){
        alert('use Enter key instead');
      },
      textEntered(){
        debugger;
       if(this.configElement.required && typeof(this.value)=='undefined') {
              alert('this field is required');
              return;
       }
       if(typeof(this.configElement.filter)!='undefined'){
            this.filterComponents = this.configElement.filter.split(':');
            switch(this.filterComponents[0]){
                case 'number':
                    if(isNaN(this.value)){
                        alert('a number is required');
                        return;
                    }
                    break;
            }
       }



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
      },

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