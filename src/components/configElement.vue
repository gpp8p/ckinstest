<template>
    <div class="configElementItem">
        <flex-checkbox v-if="this.configElement.type=='checkbox'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" @openCheckedByDefault="openCheckedByDefault" ></flex-checkbox>
        <flex-select v-if="this.configElement.type=='select'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" ></flex-select>
        <flex-color2 v-if="this.configElement.type=='color'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" ></flex-color2>
        <flex-radio v-if="this.configElement.type=='radio'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" @openCheckedByDefault="openCheckedByDefault"></flex-radio>
        <flex-font-select v-if="this.configElement.type=='fontSelect'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" ></flex-font-select>
        <flex-input-field v-if="this.configElement.type=='input'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" ></flex-input-field>
        <flex-file-upload v-if="this.configElement.type=='file'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" ></flex-file-upload>
        <save-configuration v-if="this.configElement.type=='saveConfiguration'" ref="this.configElement.element" :currentValues="currentValues" :config-element="this.configElement" @configSelected="selectionHandler_recursionComponent" ></save-configuration>
        <span  v-if="configElement.element==this.elementExpandedNow">
            <debug-config-element v-for="(configElement, index) in configElement.onClick"  :configElement="configElement" :currentValues="currentValues" :key="index" @configSelected="selectionHandler_recursionComponent" ></debug-config-element>
        </span>

    </div>
</template>


<script>
  /* eslint-disable no-console,no-debugger */

  import flexCheckbox from "../components/flexCheckbox.vue";
  import flexSelect from "../components/flexSelect.vue";
//  import flexColor from "../components/flexColor.vue";
  import flexColor2 from "../components/flexColor2.vue";
  import flexRadio from "../components/flexRadio.vue";
  import flexFontSelect from "../components/flexFontSelect.vue";
  import flexInputField from "../components/flexInputField.vue";
  import flexFileUpload from "../components/flexFileUpload.vue";
  import saveConfiguration from "../components/saveConfigurationButton.vue";
  import debugConfigElement from "../components/debugConfigElement";

  export default {
    name: "configElement",
    components:{flexCheckbox, flexSelect, flexRadio, flexFontSelect, flexInputField, flexColor2, flexFileUpload, saveConfiguration, debugConfigElement},
    props:{
      configElement: {
        type: Object,
        required: true
      },

      currentOpenExpandedElement:{
        type: String,
        required: false
      },

      currentValues:{
        type: Object,
        required: false
      },
      fieldNumber:{
          type: Number,
          required: true
      },
      onePage:{
          type: Boolean,
          required: true
      }

    },
    data(){
      return {
        showExpansion:null,
        elementExpandedNow: '',
        onClickActivateFunctions:[]
      }
    },

// eslint-disable-next-line no-console
    methods: {
      selectionHandler_recursionComponent(msg){
//        debugger;
        console.log('selectionHandler');
        console.log(msg);
        if(typeof msg[4] != 'undefined'){
          this.$emit('configSelected',[msg[0],msg[1],this.openExpander, this.closeExpander, msg[4] ]);
        }else{
          this.$emit('configSelected',[msg[0],msg[1],this.openExpander, this.closeExpander ]);
        }
      },
      openCheckedByDefault(msg){
//        debugger;
//        console.log(msg);
        this.openExpander(msg[0]);
      },
      openExpander(element){
//        debugger;
        this.elementExpandedNow=element;
      },
      closeExpander(){
//        debugger;
        this.elementExpandedNow='';
      }
    }
  };
</script>

<style scoped>
    .configElementItem {
        text-align: left;
    }

</style>