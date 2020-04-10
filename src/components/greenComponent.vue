<template>
  <div v-on:click="cellClicked" >
    {{ this.cardTitle }}
  </div>
</template>

<script>
/* eslint-disable no-console,no-debugger */
import CardBase from "../components/CardBase.vue";
export default {
  name: "greenComponent",
  extends: CardBase,
  props: {
    cardStyle: {
      type: String,
      required: true
    },
    cardId: {
      type: String,
      required: true
    },
    cardKey: {
      type: String,
      required: true
    },
    cardPosition: {
      type: Array,
      required: true
    },
    cardProperties: {
      type: String,
      required: false
    }
  },
  data() {
    return {
      cardMessage: this.getCardProps(),
      cardHasBeenSetup: false,
      cstyle: this.cardStyle,
      cardTitle:this.getCardProps(),
      styling: {},
      content: {},
      configurationCurrentValues:{},
/*
      configurationCurrentValues:{
        "backgroundTypeColor":'checked',
        "backgroundColor":"#FFFFFF",
        "fontSize":"12pt",
        "fontWeight":"bold",
        "border":"checked",
        "borderColor":"#cc0521",
        "borderSize":"medium",
        "shadow":"checked",
        "shadowSize":"10px",
        "shadowSizeSetAt":"10px",
        "shadowColor":"#BBBBBB",
        "textInput":"Have a nice Day!",
        "fontFamily":"Helvetica",
        "fontStyle":"oblique",
        "textAlign":"left",
        "color":"#0537aa"
      },
*/
      cardConfiguration: [
        {
          "label": "Card Appearance",
          "configurationElements": [

            {
              "type": "radio",
              "element": "backgroundTypeColor",
              "fieldName": "backgroundType",
              "prompt": "Background Type ?",
              "valueFrom":"backgroundTypeColor",
              "radioOptions": ['Color'],
              "onClick": [{ "type": "color", "element": "backgroundColor", "valueFrom":"backgroundColor", "prompt": "Background Color" }]
            },
            {
              "type": "radio",
              "element": "backgroundTypeImage",
              "fieldName": "backgroundType",
              "prompt": "Background Type ?",
              "valueFrom":"backgroundTypeImage",
              "radioOptions": ['Image'],
              "onClick": [{ "type": "file", "element": "backgroundImage", "prompt": "Upload -" }]
            },
            {
              "type": "checkbox", "element": "border", "valueFrom":"borderInclude", "prompt": "Include Border?",
              "onClick": [{
                "type": "select",
                "selectOptions": ['thin', 'medium', 'thick'],
                "valueFrom":"borderSize",
                "element": "borderSize",
                "prompt": "Size?"
              },
                { "type": "color", "valueFrom":"borderColor", "element": "borderColor", "prompt": "Color ?" }]
            },
            {
              "type": "checkbox", "element": "shadow", "valueFrom":"shadow", "prompt": "Shadow ?"
            },
            { "type": "checkbox", "element": "roundIncluded", "valueFrom":"roundIncluded", "prompt": "Round Corners ?" }
          ]
        },
        {
          "label": "Headline",
          "configurationElements": [
            { "type": "input", "element": "title", "valueFrom":"title", "fieldSize": "60", "prompt": "Enter Text:" }
          ]
        },
        {
          "label": "Text Attributes",
          "configurationElements": [
            { "type": "fontSelect", "valueFrom":"fontFamily", "element": "fontFamily", "prompt": "Select Font:" },
            {
              "type": "select",
              "selectOptions": ['10pt', '12pt', '18pt', '24pt', '36pt', '48pt', '72pt'],
              "valueFrom":"fontSize",
              "element": "fontSize",
              "prompt": "Size"
            },
            {
              "type": "select",
              "selectOptions": ['normal', 'bold', 'bolder', 'lighter'],
              "valueFrom":"fontWeight",
              "element": "fontWeight",
              "prompt": "Weight"
            },
            {
              "type": "select",
              "selectOptions": ['normal', 'italic', 'oblique'],
              "valueFrom":"fontStyle",
              "element": "fontStyle",
              "prompt": "Style"
            },
            {
              "type": "select",
              "selectOptions": ['left', 'center', 'right'],
              "valueFrom":"textAlign",
              "element": "textAlign",
              "prompt": "Align"
            },
            { "type": "color", "valueFrom":"color", "element": "color", "prompt": "Color:" }
          ]
        }
      ],
    }



  },

  methods: {
    cellClicked: function() {
//      debugger;
      this.styling={};
      this.loadCardConfiguration(this.cardId);
//      debugger;
      this.$emit("cardClick", [
        "cardClicked",
        this.cardKey,
        "greenComponent",
        this.setCardData,
        this.cardConfiguration,
        this.configurationCurrentValues,
      ]);
      if(this.cardTitle=='Click on this card to set it up (green card)'){
        this.cardTitle = "";
      }

    },
    refId: function() {
      return "card" + this.cardId;
    },
    getCardProps() {
//      debugger;
      if ((typeof this.cardProperties === "undefined") | (this.cardProperties == "")) {
        return "Click on this card to set it up (green card)";
      }else {
        var colonDelimiterLocatedAt= this.cardProperties.indexOf(":");
        var thisProp = this.cardProperties.substr(colonDelimiterLocatedAt+1);
        return thisProp;
      }
    }
  }
};
</script>

<style scoped>
.cardStyle {
  height: 100%;
  width: 100%;
}
</style>
