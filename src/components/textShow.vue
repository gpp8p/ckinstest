<template>
  <div class="cardStyle">
    <div class="cardHeader" v-if="displayStatus==false">
      <span class="textLeft">
        <a href="#" v-on:click="cellClicked" >Configure</a>
      </span>
      <span class="textRight">
        <a href="#"  v-on:click="editClicked" >Edit</a>
      </span>
    </div>
    <br/>

    <div class="cardBody" @click="cellClicked" v-html="this.cardData">
    </div>
  </div>
</template>

<script>
/* eslint-disable no-console,no-debugger */
import CardBase from "../components/CardBase.vue";
export default {
  name: "textShow",
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
    },
    displayStatus: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      cardMessage: this.getCardProps(),
      cardHasBeenSetup: false,
      cstyle: this.cardStyle,
      cardData:this.getCardProps(),
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
        "textShow",
        this.setCardData,
        this.cardConfiguration,
        this.configurationCurrentValues
      ]);
      if(this.cardData=='Click on this card to configure its appearence'){
        this.cardData = "";
      }

    },
    editClicked(){
      debugger;
      this.loadCardConfiguration(this.cardId);
      this.$emit('textEditor', [this.cardKey, this.setCardData,this.configurationCurrentValues, this.cardData]);
    },
    refId: function() {
      return "card" + this.cardId;
    },
    getCardProps() {
//      debugger;
      if ((typeof this.cardProperties === "undefined") | (this.cardProperties == "")) {
        return "Click on configure to set its appearence";
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
  overflow: auto;
}
.cardHeader {
  height: 10%;
  background-color: #fff722;
  color: blue;
  font-family: Geneva;
  font-size: 12px;
  font-style: normal;
  font-weight: bold;
}
.cardBody {
  height: 90%;
  margin:10px;
}
.textLeft {
  margin-left: 5px;
}

.textRight {
    margin-left: 82%;
}

.textCenter {
  text-align: center;
}


</style>
