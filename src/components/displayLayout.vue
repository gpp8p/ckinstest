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
      :displayStatus="displayStatus"
      @storeValue="processClick"
      @cardClick="cardClick"
      @textEditor="textEditor"
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
import genericCard from '../components/genericCard.vue';
  import store from "../store";


export default {
  name: "displayLayout",
  components: {genericCard},

  data() {
    return {
      csrf_token: "",
      cardInstances: [],
      gridParamDefinition: "",
      displayGrid: false,
      showCards: true,
      evmsg: '',
      layoutGrid: [],
      selectedColor: '#66bb6a',
      unSelectedColor: 'rgb(219, 170, 110)',
      newCardType: '',
      scolor: '',
      layoutConfigurationValues: {},
      displayStatus: true,

    }
  },
  mounted () {
//    debugger;
    this.reloadLayoutForDisplay(this.layoutId,sessionStorage.getItem('loggedInUserId'),sessionStorage.getItem('orgId'));
  },
  props:{
    layoutId:{
      type: String,
      required: true
    }
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
    cardDataLoaded(msg){
      this.$emit('cardDataLoaded',msg);
    },




    reloadLayoutForDisplay: function(layoutId, userId, orgId) {
      this.cardInstances = [];
      this.displayGrid=true;
      this.layoutId = layoutId;
//      this.cancelLayoutEdit();
//      console.log("reloading" + msg);
      debugger;
      axios.get('http://localhost:8000/getLayout?XDEBUG_SESSION_START=15122"', {
        params:{
          orgId:orgId,
          userId:userId,
          layoutId:this.layoutId
        }
        }).then(response => {
          // JSON responses are automatically parsed.
          debugger;
          this.cardInstances = response.data.cards;
          this.gridParamDefinition = this.layoutGridParameters(
                  response.data.layout.height,
                  response.data.layout.width,
                  response.data.layout.backgroundColor
          );
          this.LayoutPermissions = response.data.perms;
          store.commit('setPerms', response.data.perms);
          this.$emit('layoutChanged',[this.layoutId]);
        }).catch(e => {
                console.log(e);
                this.errors.push(e);
        });
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
