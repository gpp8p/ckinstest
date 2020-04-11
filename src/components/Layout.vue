<template>
  <div v-bind:style="gridParamDefinition" v-if="this.displayGrid" class="gridSection">
    <generic-card
      v-for="(instance, index) in cardInstances"
      :key="index"
      :cardType="instance.card_component"
      :card-style="instance.card_parameters.style"
      :card-id="instance.id"
      :card-key="index"
      :card-position="instance.card_position"
      :gridCss="gridParamDefinition"
      :cardProperties="instance.card_parameters.properties"
      @storeValue="processClick"
      @cardClick="cardClick"
      @configurationHasBeenSaved="configurationHasBeenSaved"
      @cardDataLoaded="cardDataLoaded"
      @linkHelperRequested="linkHelperRequested"
      ref="key"
      ></generic-card>

  </div>
</template>

<script>
  /* eslint-disable no-debugger,no-console,no-undef */

import axios from "axios";
import EventBus from '../main.js';
import genericCard from '../components/genericCard.vue';


export default {
  name: "Layout",
  components: {genericCard},
  props: {
    layoutId: {
      type: String,
      required: true
    },
    layoutCmd:{
      type: String,
      required:true
    }

  },
  watch: {
    layoutId: function(){
//      debugger;
      this.reloadLayout(this.layoutId);
    },
    layoutCmd: function(){
      debugger;
      switch(this.layoutCmd){
        case 'hide':
          this.displayGrid=false;
          break;
        case 'show':
          this.displayGrid=true;
          break;
        case 'new':
          this.$emit('cardClick',['cardClicked',null,'newLayout',this.setData,this.newLayoutConfig,{},null]);
          break;
      }
    }
  },
  data() {
    return {
      WAITINGFORCLICK:0,
      TOPLEFTCLICKED:1,
      BOTTOMRIGHTCLICKED:2,
      SELECTAREAOK:3,
      WAITINGFORNAME:4,
      WAITINGFORTYPE:5,
      WAITINGFORSUBMIT:6,
      WAITINGTOSAVE:8,
      CANCELLAYOUTUPDATE:7,
      CARDBEINGCONFIGED:8,
      csrf_token: "",
      cardInstances: [],
      gridParamDefinition: "",
      displayGrid: false,
      showCards: true,
      evmsg:'',
      topLeftClicked: 0,
      bottomRightClicked: 0,
      cstatus: 0,
      topLeftRow:0,
      topLeftCol:0,
      bottomRightRow:0,
      bottomRightCol:0,
      layoutGrid:[],
      selectedColor: '#66bb6a',
      unSelectedColor: 'rgb(219, 170, 110)',
      newCardType: '',
      scolor: '',
      layoutConfigurationValues: {},

      newLayoutConfig: [
        {
          "label": "New Layout",
          "configurationElements": [
            {
              "type": "input",
              "element": "layoutName",
              "valueFrom": "layoutName",
              "fieldSize": "32",
              "prompt": "Layout Name:"
            },
            {
              "type": "input",
              "element": "layoutDescription",
              "valueFrom": "layoutDescription",
              "fieldSize": "50",
              "prompt": "Description:"
            },
            {"type": "input",
              "element": "rows",
              "valueFrom": "rows",
              "fieldSize": "5",
              "prompt": "Rows:"
            },
            {
              "type": "input",
              "element": "cols",
              "valueFrom": "cols",
              "fieldSize": "5",
              "prompt": "Columns:"
            }
          ]
        }
      ]

    };
  },
  mounted () {
    EventBus.$on('load-layout', function () {
//    console.log('event bus recieved event');
    });
  },
  methods: {
    layoutGridParameters(height, width, backgroundColor) {
      var heightSize = (95 / height).toFixed(2);
      var widthSize = (98 / width).toFixed(2);
      var gridHeightCss = "grid-template-rows: ";
      var gridWidthCss = "grid-template-columns: ";
      var x = 0;
      for (x = 0; x < height; x++) {
        gridHeightCss = gridHeightCss + heightSize + "% ";
      }
      for (x = 0; x < width; x++) {
        gridWidthCss = gridWidthCss + widthSize + "% ";
      }
      var gridCss =
        "display: grid; grid-gap: 3px; background-color: "+backgroundColor+"; height: 90vh; color: #ffcd90; " +
        gridHeightCss +
        ";" +
        gridWidthCss +
        ";";
      return gridCss;
    },
    hideGrid(){
      this.displayGrid=false;
      this.showCards=false;
    },
    getCardProperties(){
      console.log('getCardProperties');
    },
    showGrid(){
      this.displayGrid=true;
    },
    cardDataLoaded(msg){
      this.$emit('cardDataLoaded',msg);
    },
    linkHelperRequested(){
      this.$emit("linkHelperRequested");
    },
    reloadLayout: function(msg) {
//      debugger;
      this.cardInstances = [];
      this.displayGrid=true;
      this.layoutId = msg;
      this.cancelLayoutEdit();
//      console.log("reloading" + msg);
      axios
        .get("http://localhost:8000/getLayout?layoutId=" + this.layoutId+"&&XDEBUG_SESSION_START=15122")
        .then(response => {
          // JSON responses are automatically parsed.
//          debugger;
          this.cardInstances = response.data.cards;
          this.gridParamDefinition = this.layoutGridParameters(
            response.data.layout.height,
            response.data.layout.width,
            response.data.layout.backgroundColor
          );
// build a blank layout using the dimensions of the layout loaded
          var newBlankLayout = this.makeBlankLayout(response.data.layout.height,response.data.layout.width, response.data.layout.description, response.data.layout.menu_label, response.data.layout.backgroundColor)
//          console.log(newBlankLayout);
          var layoutGrid = newBlankLayout[3];
          var cardsToDelete = [];
          for(var thisCardIndex=0; thisCardIndex<this.cardInstances.length;thisCardIndex++){
            var thisCard = this.cardInstances[thisCardIndex];
            var cardTopLeftRow = thisCard.card_position[0]-1;
            var cardTopLeftColumn = thisCard.card_position[1]-1;
            var cardBottomRightRow = (cardTopLeftRow+thisCard.card_position[2]);
            var cardBottomRightColumn = (cardTopLeftColumn+thisCard.card_position[3]);
            console.log('cardId:'+thisCard.id+' cardTopLeftRow:'+cardTopLeftRow+' cardTopLeftColumn:'+cardTopLeftColumn+' cardBottomRightRow:'+cardBottomRightRow+' cardBottomRightColumn:'+cardBottomRightColumn)
// build cardsToDelete by taking it from indexes in layoutGrid within the dimensions of the card to show
            for(var r = cardTopLeftRow; r<cardBottomRightRow; r++){
              for(var c = cardTopLeftColumn; c<cardBottomRightColumn; c++){
                cardsToDelete.push(layoutGrid[r][c]);
              }
            }
//            console.log(thisCard);
//            debugger;
          }
//          debugger;

// set the toDelete flag in the blank cards for everything in the cardsToDelete list
          var blankLayout = newBlankLayout[1].cards;
//          debugger;
          for(var d=0;d<cardsToDelete.length;d++){
            var indexOfCardToDelete = cardsToDelete[d];
            console.log(indexOfCardToDelete);
            blankLayout[indexOfCardToDelete].toDelete=true;
          }
// copy all the cards in blankLayout that are not to be deleted
          var newCardInstances = [];
          for(c = 0;c<blankLayout.length;c++){
            if(!blankLayout[c].toDelete){
              newCardInstances.push(blankLayout[c]);
            }
          }
// add the cards coming from the db
          for(thisCardIndex=0; thisCardIndex<this.cardInstances.length;thisCardIndex++){
            newCardInstances.push(this.cardInstances[thisCardIndex]);
          }
// make cardInstances equal to newCardInstances
          this.cardInstances=newCardInstances;

        })
        .catch(e => {
          console.log(e);
          this.errors.push(e);
        });

    },

    reloadLayoutForDisplay: function(msg) {
      this.cardInstances = [];
      this.displayGrid=true;
      this.layoutId = msg;
      this.cancelLayoutEdit();
//      console.log("reloading" + msg);
      axios
        .get("http://localhost:8000/getLayout?layoutId=" + this.layoutId+"&&XDEBUG_SESSION_START=15122")
        .then(response => {
          // JSON responses are automatically parsed.
//          debugger;
          this.cardInstances = response.data.cards;
          this.gridParamDefinition = this.layoutGridParameters(
            response.data.layout.height,
            response.data.layout.width,
            response.data.layout.backgroundColor
          );
        })
        .catch(e => {
          console.log(e);
          this.errors.push(e);
        });
    },


    reloadBlankLayout(blankCardInstances){
//      console.log('reloadBlankLayout');
      this.displayGrid=true;
      this.cardInstances = blankCardInstances.cards;
      this.gridParamDefinition = this.layoutGridParameters(
        blankCardInstances.layout.height,
        blankCardInstances.layout.width,
        blankCardInstances.layout.backgroundColor
      );
//      console.log('gridParams:'+this.gridParamDefinition);

    },
    cancelLayoutEdit(){
//      console.log('noButton clicked');
      this.cstatus = this.WAITINGFORCLICK;
      this.scolor = this.unSelectedColor;
//      debugger;
      if(this.bottomRightCol==0 && this.bottomRightRow==0){
        this.fillInOneCell(this.cardInstances, this.topLeftRow, this.topLeftCol, this.unSelectedColor);
      }else{
        this.fillSelectedCells(this.cardInstances,this.topLeftCol,this.topLeftRow,this.bottomRightCol,this.bottomRightRow, this.unSelectedColor);
      }

    },
    fillInCell(item, index, arr){
//      debugger;
      var thisCardCol = arr[index].card_position[1];
      var thisCardRow = arr[index].card_position[0];
      var topLeftCol = arr[this.topLeftClicked].card_position[1];
      var topLeftRow = arr[this.topLeftClicked].card_position[0];
      var bottomRightCol = arr[this.bottomRightClicked].card_position[1];
      var bottomRightRow = arr[this.bottomRightClicked].card_position[0];
//      console.log(thisCardCol+'-'+thisCardRow+'-'+topLeftCol+'-'+topLeftRow+'-'+bottomRightCol+'-'+bottomRightRow);
      if(thisCardCol >= topLeftCol && thisCardRow >= topLeftRow && thisCardCol <= bottomRightCol && thisCardRow <= bottomRightRow){
//        console.log(item.id);
        this.$refs.key[index].$el.style.backgroundColor=this.scolor;
      }else{
//        console.log(item.id);
//        console.log(thisCardCol+'-'+thisCardRow+'-'+topLeftCol+'-'+topLeftRow+'-'+bottomRightCol+'-'+bottomRightRow);
      }
    },
    checkClickPos(brClickRow, brClickCol, tlSelectRow, tlSelectCol){
//      debugger;
      if((brClickRow<tlSelectRow)||(brClickCol<tlSelectCol)){
        return false;
      }else{
        return true;
      }


    },
    fillInOneCell(arr, cellRow, cellColumn, colorToFill){
      for(var i = 0; i<arr.length; i++){
        var thisCardCol = arr[i].card_position[1];
        var thisCardRow = arr[i].card_position[0];
        if(thisCardCol==cellColumn && thisCardRow == cellRow){
//          console.log('card matched');
          this.$refs.key[i].$el.style.backgroundColor=colorToFill;
        }

      }
    },
    fillSelectedCells(arr,tlCol,tlRow,brCol,brRow, colorToFill){
      var topLeftCol = tlCol;
      var topLeftRow = tlRow;
      var bottomRightCol = brCol;
      var bottomRightRow = brRow;
      for(var i = 0; i<arr.length; i++){
        var thisCardCol = arr[i].card_position[1];
        var thisCardRow = arr[i].card_position[0];
//        console.log('thisCardCol:'+thisCardCol+'topLeftCol:'+topLeftCol+'thisCardRow:'+thisCardRow+'topLeftRow:'+topLeftRow);
//        console.log('thisCardCol:'+thisCardCol+'bottomRightCol:'+bottomRightCol+'thisCardRow:'+thisCardRow+'bottomRightRow:'+bottomRightRow);
//        console.log(arr[i].id);
//        console.log('-----------------------------')
        if(thisCardCol >= topLeftCol && thisCardRow >= topLeftRow && thisCardCol <= bottomRightCol && thisCardRow <= bottomRightRow){
//          console.log('card matched');
          this.$refs.key[i].$el.style.backgroundColor=colorToFill;
        }

      }
//      console.log('done');
    },
    cardClick(msg){
//      debugger;
//      var setMsg = msg[0][3];
//      var cKey = setMsg('here is a message');
//      console.log(cKey);
//      console.log("Card Click - "+msg);
//      debugger
      var instanceNum = msg[0][1];
      msg[0].push(this.$refs.key[instanceNum].$el);
      msg[0].push(this.cardInstances[instanceNum]);
      this.$emit('cardClick', msg[0])
//      debugger;
    },
    processClick(msg){
      console.log('editGrid2 gets storeValue-'+msg);
//      debugger;
      var cardThatWasClicked = this.findCard(msg[0]);
//      console.log('cardThatWasClicked:'+cardThatWasClicked);
      switch(this.cstatus){
        case this.CARDBEINGCONFIGED:
          break;
        case this.WAITINGFORCLICK:
          this.topLeftClicked=msg[0];
          this.topLeftRow = this.cardInstances[cardThatWasClicked].card_position[0];
          this.topLeftCol = this.cardInstances[cardThatWasClicked].card_position[1];
//          debugger;
          this.cstatus=this.TOPLEFTCLICKED;
          this.$refs.key[cardThatWasClicked].$el.style.backgroundColor='#66bb6a';
          this.$emit('storeValue', ['topLeft', this.topLeftRow,this.topLeftCol ]);
          break;
        case this.TOPLEFTCLICKED:
          this.bottomRightClicked = msg[0];
          var brClickRow = this.cardInstances[cardThatWasClicked].card_position[0];
          var brClickCol = this.cardInstances[cardThatWasClicked].card_position[1];
          if(this.checkClickPos(brClickRow, brClickCol, this.topLeftRow, this.topLeftCol)){
            this.bottomRightRow = this.cardInstances[cardThatWasClicked].card_position[0];
            this.bottomRightCol = this.cardInstances[cardThatWasClicked].card_position[1];
            this.cstatus=this.BOTTOMRIGHTCLICKED;
            this.$refs.key[cardThatWasClicked].$el.style.backgroundColor='#66bb6a';
            this.scolor = this.selectedColor;
//            this.cardInstances.forEach(this.fillInCell);
            this.fillSelectedCells(this.cardInstances,this.topLeftCol,this.topLeftRow,this.bottomRightCol,this.bottomRightRow, '#66bb6a');
            this.$emit('storeValue', ['bottomRight', this.bottomRightRow,this.bottomRightCol ]);
          }else{
            this.$emit('storeValue', ['error', 'You must click and to the right',0 ]);
          }
          break;
        case this.BOTTOMRIGHTCLICKED:
          this.cstatus = this.WAITINGFORNAME;
          break;
        case this.WAITINGFORNAME:
          this.cstatus = this.WAITINGFORTYPE;
          break;
      }
    },
    findCard(cardId){
      for(var i=0;i<this.cardInstances.length;i++){
        if(this.cardInstances[i].id == cardId){
          return i;
        }
      }
    },


//    insertCardsIntoBlankGrid(layoutCards){
//     var newBlankLayout = this.makeBlankLayout(layoutCards.layout.height, layoutCards.layout.width, layoutCards.layout.description, layoutCards.layout.menu_label);
//     var removalList = [];
//      for(var card = 0;card<layoutCards.cards.length;card++){

//      }
//    },

    setData(cardData, cardDataElement){
      switch (cardDataElement){
        case 'layoutName':
          this.layoutConfigurationValues['layoutName'] = cardData;
          break;
        case 'layoutDescription':
          this.layoutConfigurationValues['layoutDescription'] = cardData;
          break;
        case 'rows':
          this.layoutConfigurationValues['rows'] = cardData;
          break;
        case 'cols':
          this.layoutConfigurationValues['cols'] = cardData;
          break;
        case 'backgroundColor':
          break;
        case 'save':
          break;
      }
    },


    createBlankLayout(height,width, description, menu_label){
//      console.log('createBlankLayout:'+height+' '+width);
      this.$emit('storeValue', this.makeBlankLayout(height,width, description, menu_label));
    },

    makeBlankLayout(height,width, description, menu_label, backgroundColor){
//      debugger;
      this.layoutGrid = [];
      var newCards = [];
//      var newCardId=1;
      var newCardId=0;
      height++;
      width++;
      for(var h=1;h<height;h++){
        var gridRow = [];
        for(var w = 1; w<width; w++){
          var c=this.createBlankCardInstance(h,w,1,1,newCardId);
          newCards.push(c);
          gridRow.push(newCardId);
          newCardId++;
        }
        this.layoutGrid.push(gridRow);
      }
      var newLayout = {cards: newCards, layout: {description:description, menu_label: menu_label, height: (height-1), width:(width-1)}};
      var newGridParameters = this.layoutGridParameters(height, width, backgroundColor);
      return ['newBlankGrid', newLayout, newGridParameters, this.layoutGrid ];
    },

    computeGridCss(row, col, height, width){
//        debugger;
      var startRow = row;
      var startColumn = col;
      var endRow=0;
      var endCol=0;
      if(height==1){
        endRow = row;
      }else{
        endRow = row+height;
      }
      endCol=startColumn+width;
      var thisCss = "grid-area:"+startRow+"/"+startColumn+"/"+endRow+"/"+endCol;
      return thisCss;

    },


    createBlankCardInstance(row, col, height, width, id){
//      console.log('createBlankCardInstance:'+row+' '+col+' '+height+' '+width+ ' '+id);
      var thisGridCss = this.computeGridCss(row, col, height, width);
      var thisCardStyle = thisGridCss+";"+"background-color:#DBAA6E;color:blue;";
      var thisInstance = {card_component: 'simpleCard', card_position: [row,col,height,width], id:id, toDelete: false, card_parameters: {style: thisCardStyle}};
      return thisInstance;

    },

    freezeCellSelection(){
      this.cstatus=this.CARDBEINGCONFIGED;
    },
    unlockCellSelection(){
      this.cstatus = this.WAITINGFORCLICK;
    },
    unfreezeCellSelection(){
      this.cstatus = this.WAITINGFORCLICK;
      this.scolor = this.unSelectedColor;
      this.fillSelectedCells(this.cardInstances,this.topLeftCol,this.topLeftRow,this.bottomRightCol,this.bottomRightRow, this.unSelectedColor);
    },
    configurationHasBeenSaved(){
      this.$emit('configurationHasBeenSaved');
    }




  }
};
</script>

<style scoped>
  .gridSection {
    height:80vh;
    width:96vw;
  }
  .prompt {
    font-size: medium;
    color: blue;
    margin-bottom: 5px;
    text-align: left;
  }



</style>
