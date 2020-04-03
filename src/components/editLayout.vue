<template>
    
</template>

<script>
    import axios from "axios";
    export default {
        name: "editLayout",
        data (){
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

              layoutId:0,
              cardInstances:[],
              gridParamDefinition:'',
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

          }
        },
        methods:{
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



        }
    }
</script>

<style scoped>

</style>