<template>
    <span>
        <span v-for="(choice, index) in this.radioOptions" :key="index">
            <input type="radio" name=this.fieldName @click="choiceSelected(index)" value="this.fieldValue" :checked="checked" @change="valueChanged"  />{{choice}}
        </span>
    </span>
</template>

<script>
  /* eslint-disable no-console,no-debugger */

  export default {
    name: "flexRadio",
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
      return {
        radioOptions: this.configElement.radioOptions,
        fieldName: this.configElement.fieldName,
        fieldValue: this.configElement.element,
        radioValue: '',
        checked: this.getCurrentValue()
//        selectOptions: []
      }
    },
    watch:{
      currentValues: function(){
//          debugger;
        this.checked = this.getCurrentValue();
      }
    },


// eslint-disable-next-line no-console
    methods:{
      valueChanged(){
//        console.log('radio value changed-'+this.fieldValue);
      },
      choiceSelected(idx){
        this.$emit('configSelected', [this.configElement.element,'activated', this.radioOptions[idx]]);
      },
      getCurrentValue(){
//        debugger;
        var currentValuesEntries = Object.entries(this.currentValues);
        for(var v = 0;v<currentValuesEntries.length;v++){
//          debugger;
          var thisCurrentValue = currentValuesEntries[v];
          if(thisCurrentValue[0]==this.configElement.valueFrom){
            var currentValuesEntriesParts = currentValuesEntries[v][1];
            if(currentValuesEntriesParts=='checked'){
              this.$emit('openCheckedByDefault',[this.configElement.element]);
              return true;
            }else{
              return false;
            }
          }
        }
      }

    }
  };
</script>

<style scoped>

</style>