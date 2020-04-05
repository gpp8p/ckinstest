<template>
    <span>
        <input type="checkbox" @click="this.clicked" :checked="checked" value="on" /> - {{this.configElement.prompt}}
    </span>
</template>

<script>
  /* eslint-disable no-debugger */

  //  import flexConfigComponent from "../components/flexConfigComponent.vue";
//    import dummyConfigComponent from "../components/dummyConfigComponent.vue";
  export default {
    name: "flexCheckbox",
//    components: {'flexConfigComponent':flexConfigComponent},
//      components: {dummyConfigComponent},
    props:{
      configElement: {
        type: Object,
        required: true
      },
      currentValues:{
        type: Object,
        required: false
      }

// eslint-disable-next-line no-debugger
    },
    data(){
        return {
          showExpansion:null,
          sizePrompt:'Size',
          checked: this.getCurrentValue()
        }
    },

    watch:{
        currentValues: function(){
//          debugger;
          this.checked = this.getCurrentValue();
        }
    },

    methods:{
      clicked(event){
        if (event.target.checked){
          this.showExpansion=true;
          this.checked=true;
          this.$emit('configSelected', [this.configElement.element,'activated']);
        }else{
          this.showExpansion=false;
          this.checked=false;
          this.$emit('configSelected', [this.configElement.element,'deactivated']);
        }
      },
      isClicked(){
//        debugger;
        return this.checked;
      },
      getCurrentValue(){
//        debugger;
        var currentValuesEntries = Object.entries(this.currentValues);
        for(var v = 0;v<currentValuesEntries.length;v++){
          var thisCurrentValue = currentValuesEntries[v];
          if(thisCurrentValue[0]==this.configElement.valueFrom){
            var currentValuesEntriesParts = currentValuesEntries[v][1].split(":");
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