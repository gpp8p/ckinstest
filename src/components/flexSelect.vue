<template>
   <span>

        <select class="selectStyle" v-model="this.selectValue" ref="thisSelect" @change="broadcastValue(event)">
            <option class="optionStyle" value="" disabled selected>{{this.configElement.prompt}}</option>
            <option class="optionStyle" v-for="(val, index) in selectionOptions" :selected="val==selectedValue" :key="index" v-bind:value="val" >{{ val }}</option>
        </select>
       <span> - {{this.configElement.prompt}}</span>
    </span>

</template>

<script>
  /* eslint-disable no-debugger */

  export default {
    name: "flexSelect",
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
    watch:{
      currentValues: function(){
//          debugger;
        this.selectValue = this.getCurrentValue();
      }
    },


    data(){
      return {
        selectionOptions: this.configElement.selectOptions,
        selectValue: this.getCurrentValue(),
//        selectOptions: []
      }
    },
    methods: {
      broadcastValue(){
//        console.log(evt);
//        debugger;
        this.$emit('configSelected', [this.configElement.element, this.$refs.thisSelect.value]);
      },
      getCurrentValue(){
//        debugger;
        if(typeof(this.currentValues[this.configElement.element])=='undefined'|this.currentValues[this.configElement.element]=='default'){
          return '';
        }else{
          return this.currentValues[this.configElement.element];
  //        var currentValuesEntriesParts = this.currentValues[this.configElement.element].split(":");
  //        return currentValuesEntriesParts[1].replace(';','');
        }
      }
    }
  };
</script>

<style scoped>
    .selectStyle {
        background: #DBAA6E;
        color:blue;
        font-weight: bold;
        font-size: 12px;
        margin-bottom: 3px;
    }
    .optionStyle {
        background: #DBAA6E;
        color:blue;
        width: 60px;
    }

</style>