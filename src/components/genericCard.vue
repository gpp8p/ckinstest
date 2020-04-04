<template >
<span v-bind:style='cardStyle' v-on:click="processClick" id=cardId>
        <green-component  v-if="cardType=='greenComponent'" class="genericCardStyle"
                          :card-style=cardStyle
                          :card-id=cardId
                          :card-key=cardKey
                          :card-position=cardPosition
                          :cardProperties = cardProperties
                          @cardClick="processCardClick"
                          @configurationHasBeenSaved="configurationHasBeenSaved"
                          @cardDataLoaded="cardDataLoaded"
                          @cardPropertySet="cardPropertySet"
                          ref="cardKey"
        ></green-component>
        <ckeditor-component  v-if="cardType=='cktestComponent'" class="genericCardStyle"
                          :card-style=cardStyle
                          :card-id=cardId
                          :card-key=cardKey
                          :card-position=cardPosition
                          :cardProperties = cardProperties
                          @cardClick="processCardClick"
                          @configurationHasBeenSaved="configurationHasBeenSaved"
                          @cardDataLoaded="cardDataLoaded"
                          @cardPropertySet="cardPropertySet"
                          @linkHelperRequested="linkHelperRequested"
                          ref="cardKey"
        ></ckeditor-component>


</span>




</template>


<script>
  /* eslint-disable no-console,no-debugger */

  import GreenComponent from "./greenComponent";
  import GenericCardBase from "./GenericCardBase.vue";
  import CkeditorComponent from "./ckeditorComponent";

  export default {
    name: "genericCard",
    extends: GenericCardBase,
    components: {CkeditorComponent,  GreenComponent},
    props: {
      cardType: {
        type: String,
        required: true
      },
      cardStyle: {
        type: String,
        required: true
      },
      cardId: {
        type: String,
        required: true
      },
      cardKey:{
        type: String,
        required: true
      },
      cardPosition: {
        type: Array,
        required: true
      },
      gridCss: {
        type: String,
        required: true
      },
      cardProperties: {
        type: String,
        required: false
      }
    },
    data(){
      return {

      }
    },
    methods: {
      processClick(msg){
//        debugger;
        if(this.hasChildern(this.$refs)){
          console.log('hasChildern is true');
        }else{
          console.log('no childern');
          console.log('generic card recieved a click:'+msg);
          this.$emit('storeValue', [this.cardId])
        }
      },
      configurationHasBeenSaved(){
        this.$emit('configurationHasBeenSaved');
      },
      processCardClick(msg){
        console.log('blankComponent card recieved a click:'+msg);
        this.$emit('cardClick', [msg])
      },
      hasChildern(refs){
        for(var o in refs ){
          return true;
        }
        return false;
      },
      cardDataLoaded(msg){
        this.$emit('cardDataLoaded', msg);
      },
      cardPropertySet(msg){
        console.log(msg);
        this.setCardData(msg[0], msg[1]);
      },
      linkHelperRequested(){
        this.$emit('linkHelperRequested');
      }

    }
  };
</script>

<style scoped>
    .genericCardStyle{
        height:100%;
        width:100%;

    }
</style>