<template>
    <span class="layoutScreen">
        <div v-if="this.floatingView==this.VIEW_FLOATING_CONFIG" id="infoi" v-bind:style='styleObject' >
            <config-component v-if="this.draggedComponent=='configComponent'"
                          @newLocation="this.setNewLocation"
                          @startDrag="this.startDrag"
                          :currentValues=this.cardCurrentConfigurationValues
                          :configElement=this.cardConfigurationElements
                          @configSelected="configSelected" >
            ></config-component>
            <SimpleNewCard
                    v-if="this.draggedComponent=='simpleNewCard'"
                    @newLocation="this.setNewLocation"
                    @startDrag="this.startDrag"
                    @configSelected="this.configSelected"
                    @saveNewCard="this.saveNewCard"
            >
            </SimpleNewCard>

            <SimpleNewLayout
                    v-if="this.draggedComponent=='simpleNewLayout'"
                    @newLocation="this.setNewLocation"
                    @startDrag="this.startDrag"
                    @configSelected="this.configSelected"
                    @saveNewLayout="this.saveBlankLayout"
            ></SimpleNewLayout>
            <simpleCkEditor
                    v-if="this.draggedComponent=='simpleCkEditor'"
                    :updateCallback="this.updateCallback"
                    :cardData="this.cardData"
                    :layoutLink="this.layoutLink"
                    @newLocation="this.setNewLocation"
                    @startDrag="this.startDrag"
                    @configSelected="this.configSelected"
            ></simpleCkEditor>
            <div class="linkHelperStyle" >
                <layout-links-helper v-if="showLinkHelper" @configSelected="configSelected" @layoutSelected="this.layoutLinkSelected"></layout-links-helper>
            </div>

        </div>

      <section class="navbar">
        <div v-if="this.navBarView==this.VIEW_TOP_MENU">
            <menu-component :items="this.topMenuItems" @menuSelection="menuSelection"></menu-component>

        </div>
        <div v-if="this.navBarView==this.VIEW_GRID_MENU">
            <menu-component :items="this.editMenuItems" @menuSelection="menuSelection"></menu-component>
        </div>
        <div v-if="this.navBarView==this.VIEW_DISPLAY_LAYOUT">
            <menu-component :items="this.displayMenuItems" @menuSelection="menuSelection"></menu-component>
        </div>

      </section>
      <section class="content">
        <span v-if="this.contentView==this.VIEW_TEST_CKEDITOR" class="editor">
          <ckeditor-component></ckeditor-component>
        </span>
        <div v-if="this.contentView==this.VIEW_LAYOUT_LIST">
            <layout-list @layoutSelected="layoutSelected"></layout-list>
        </div>
        <div v-if="this.contentView==this.VIEW_DISPLAY_LAYOUT">
            <display-layout :layoutId="this.displayLayoutId"></display-layout>
        </div>
        <layout  :layoutId="selectedLayoutId" :layoutCmd="layoutCmd" ref="editGrid" @cardClick="cardClick" @textEditor="textEditor" @layoutMessage="this.layoutMessage" @configurationHasBeenSaved="configurationHasBeenSaved" @cardDataLoaded="cardDataLoaded" @linkHelperRequested="linkHelperRequested" @newLayoutSaved="newLayoutSaved" @cardSaved="cardSaved" ></layout>
      </section>
    </span>
</template>

<script>
  import CkeditorComponent from './components/ckeditorComponent.vue'
  import CKEditor from '@ckeditor/ckeditor5-vue';
  import Vue from 'vue';
  import layoutList from './components/LayoutList.vue';
  import menuComponent from './components/menuComponent.vue'
  import Layout from "./components/Layout";
  import configComponent from "./components/configComponent.vue";
  import SimpleNewLayout from "./components/SimpleNewLayout.vue";
  import SimpleNewCard from "./components/SimpleNewCard.vue";
  import SimpleCkEditor from "./components/SimpleCkEditor.vue";
  import displayLayout from "./components/displayLayout";
  import layoutLinksHelper from "./components/LayoutLinksHelper.vue";


//  import axios from 'axios';


  Vue.use( CKEditor );

  export default {
    name: 'app',
    components: {Layout, layoutList, CkeditorComponent, menuComponent, configComponent, SimpleNewLayout, SimpleNewCard, SimpleCkEditor, displayLayout, layoutLinksHelper },
    mounted: function() {
          this.navBarView = this.VIEW_TOP_MENU;
          if(typeof(this.displayLayoutId)=='undefined'){
              this.contentView = this.VIEW_LAYOUT_LIST;
          }else{
              this.contentView = this.VIEW_DISPLAY_LAYOUT;
              this.navBarView = this.VIEW_DISPLAY_LAYOUT;
          }

          console.log('layoutId='+this.displayLayoutId);
     },
    data () {
        return {
            VIEW_TOP_MENU: 0,
            VIEW_CREATE_LAYOUT: 1,
            VIEW_GRID_MENU: 2,
            VIEW_CARD_MENU: 3,
            VIEW_HEADLINE_CONFIG: 4,
            VIEW_FLOATING_CONFIG: 5,
            VIEW_LAYOUT_LIST: 6,
            VIEW_TEST_CKEDITOR: 7,
            VIEW_DEBUG: 8,
            VIEW_DISPLAY_LAYOUT: 9,
            contentView: this.VIEW_TOP_MENU,
            navBarView: this.VIEW_TOP_MENU,
            floatingView: this.VIEW_TOP_MENU,
            showCkTest: false,
            allLayouts: [],
            topMenuItems: ['New Layout', 'UserAdministration'],
            editMenuItems: ['Page Preview', 'Layout List'],
            displayMenuItems: ['Go Back', 'Layout List'],
            selectedLayoutId: '',
            layoutCmd: '',



            componentLeft: 0,
            componentTop: 0,
            styleObject: {
                left: '200px',
                top: '200px',
            },
            offsetLeft: 0,
            offsetTop: 0,
            showDrg: true,
            draggedComponent: '',

            cardCurrentConfigurationValues:{},
            configCard: false,
            cardTypeBeingConfigured: '',
            instancePositionBeingConfigured:0,
            screenElementBeingConfigured: {},
            cardDataFunction:null,
            cardConfigurationElements:{},
            onePage:false,

            newCardCoords: [],
            updateCallback: null,
            cardData: '',
            layoutLink:'',

            showLinkHelper:false,

            displayLayoutId: this.$route.params.layoutId
        }
    },


    methods:{
        layoutSelected(msg){
            console.log('layoutSelected');
            this.navBarView = this.VIEW_GRID_MENU;
            this.contentView = this.VIEW_GRID_MENU;
            this.editMenuItems = ['Page Preview', 'Layout List'];
            this.layoutCmd='show:'+msg[0];
            this.selectedLayoutId=msg[0];
        },
        configSelected(msg){
            debugger;
            switch(msg[0]){
                case 'cancel':
                    this.navBarView = this.VIEW_TOP_MENU;
                    this.contentView = this.VIEW_LAYOUT_LIST;
                    this.draggedComponent="";
                    this.layoutCmd = 'hide';
                    break;
                case 'hideConfigComponent':
                    this.draggedComponent="";
                    break;
                case 'hideCkDialog':
                    this.draggedComponent="";
                    break;
                case 'layoutLinkHelperRequested':
                    this.showLinkHelper=true;
                    break;
                case 'layoutLinkHelperCanceled':
                    this.showLinkHelper=false;
                    break;
                default:
                    this.cardDataFunction(msg[1], msg[0]);
                    break;

            }
            console.log(msg);
        },
        createLayout(){
            console.log('createLayout summoned');
        },
        menuSelection(msg){
            debugger;
            switch(msg[0]){
                case 'Layout List':
                    this.navBarView = this.VIEW_TOP_MENU;
                    this.contentView = this.VIEW_LAYOUT_LIST;
                    this.layoutCmd='hide';
                    this.$refs.editGrid.hideGrid();
                    break;
                case 'New Layout':

                    this.floatingView = this.VIEW_FLOATING_CONFIG;
                    this.cardCurrentConfigurationValues={};
                    this.cardConfigurationElements = this.newLayoutConfig;
                    this.onePage=true;
                    this.draggedComponent='newLayout';
                    this.draggedComponent='simpleNewLayout';
//                    this.layoutCmd = 'new';
                    break;
                 case 'CkEditor':
                     this.floatingView = this.VIEW_FLOATING_CONFIG;
                     this.draggedComponent='simpleCkEditor';
                     break;
                  case 'Page Preview':
                      this.navBarView = this.VIEW_GRID_MENU;
                      this.contentView = this.VIEW_GRID_MENU;
                      this.editMenuItems = ['Edit', 'Layout List'];
                      this.layoutCmd = 'display';
                      break;
                  case 'Edit':
                      this.navBarView = this.VIEW_GRID_MENU;
                      this.contentView = this.VIEW_GRID_MENU;
                      this.editMenuItems = ['Page Preview', 'Layout List'];
                      this.layoutCmd='show:'+this.selectedLayoutId;
                      break;
                  case 'Go Back':
                      this.$router.go(-1);
                      break;



            }
        },
        setNewLocation(msg){
// eslint-disable-next-line no-debugger
//      debugger;
            this.componentLeft = msg[0]-this.offsetLeft;
            this.componentTop = msg[1]-this.offsetTop;

            this.styleObject.left = this.componentLeft+'px';
            this.styleObject.top= this.componentTop+'px';

        },
        startDrag(msg){
//        debugger;
            console.log(msg);
            this.offsetLeft = msg[0]-this.componentLeft;
            this.offsetTop = msg[1]-this.componentTop;


        },
        cardDataLoaded(msg){
          console.log(msg);
          this.cardCurrentConfigurationValues=msg[1];
        },
        textEditor(msg){
          console.log(msg);
            debugger;
            this.updateCallback = msg[0][1];
            this.cardData = msg[0][3];
            this.floatingView = this.VIEW_FLOATING_CONFIG;
            this.draggedComponent='simpleCkEditor';
        },
        cardClick(msg){
 //           debugger;
            console.log(msg);
/*
            if(msg[4][0].onePage){
                this.onePage=true;
            }else{
                this.onePage=false;
            }
 */
            this.configCard=true;
            this.cardTypeBeingConfigured = msg[2]
            this.instancePositionBeingConfigured = msg[1];
            this.screenElementBeingConfigured = msg[4];
            this.cardDataFunction = msg[3];
            this.cardConfigurationElements=msg[4];
            this.cardCurrentConfigurationValues=msg[5];
            this.floatingView = this.VIEW_FLOATING_CONFIG;
            this.draggedComponent = 'configComponent';
        },
        saveBlankLayout(msg){
//            console.log(msg);
            this.selectedLayoutId = msg[0];
//            this.layoutCmd = 'blankLayout'+':'+msg[1]+':'+msg[2]+':'+msg[3]+':'+msg[4]+':'+msg[5];
            this.navBarView = this.VIEW_GRID_MENU;
            this.contentView = this.VIEW_GRID_MENU;
            this.draggedComponent = '';
            this.layoutCmd='show:'+msg[0];
        },
        newLayoutSaved(){
            debugger;
            this.navBarView = this.VIEW_GRID_MENU;
            this.contentView = this.VIEW_GRID_MENU;
            this.draggedComponent="";
            this.layoutCmd='show';
        },
        saveNewCard(msg){
            debugger;
          console.log('saveNewCard',msg);
          this.layoutCmd="saveNewCard:"+msg[0]+":"+msg[1];
        },
        cardSaved(msg){
          console.log('cardSaved:',msg);
          this.navBarView = this.VIEW_GRID_MENU;
          this.contentView = this.VIEW_GRID_MENU;
          this.draggedComponent="";
          this.layoutCmd='show:'+msg[0];
        },
        layoutMessage(msg){
            debugger;
//            console.log(msg);
            switch(msg[0]){
                case 'topLeft':
                    this.newCardCoords.push(msg[1]);
                    this.newCardCoords.push(msg[2]);
                    break;
                case 'bottomRight':
                    this.newCardCoords.push(msg[1]);
                    this.newCardCoords.push(msg[2]);
                    this.floatingView = this.VIEW_FLOATING_CONFIG;
                    this.draggedComponent='simpleNewCard';
                    break;

            }
        },
        layoutLinkSelected(msg){
//            debugger;
//            console.log(msg);
            this.layoutLink=msg[0];
            this.showLinkHelper=false;
        },
        goBack(){
            this.$router.go(-1);
        }
    }
  }
</script>

<style>

  section {

    padding: 0 0%;
    display: table;
    margin: 0;
    height: 98vh;

  }
  .layoutScreen {
    position: relative;
  }
  .content {
    margin-left: 2px;
    margin-top: 2px;
    background-color: #dbddd0;
    height:85vh;
    width:97vw;
    border-radius: 4px;
    border-width: 2px;
    border-style: solid;
    border-color: #0a3aff;
    padding-top: 16px;
    padding-left: 5px;
  }
  .navbar {
    margin-left: 2px;
    background-color: #ffcd90;
    height:5vh;
    width:97vw;
    border-radius: 4px;
    border-width: 2px;
    border-style: solid;
    border-color: #0a3aff;
    text-align: left;
    padding-top: 8px;
    padding-left: 5px;
  }
  .layoutMenu {
    display: flex;
    justify-content: space-evenly;
    height: 100%;
    align-items: baseline;
    margin-top: 6px;
  }
  .layoutMenuItem {
    background-color: #ffcd90;
    font-family: Arial;
    font-size: 17px;
    padding: 1px;
  }
  .layoutMenuItem:hover {
    background-color: #fff722;
    color:red;
  }
  .editor {
      width:100%;
  }
  #infoi {
    width: 100%;
    height: 100%;
    position: absolute;
  }
  #infoi {
    z-index: 10;
  }
  .linkHelperStyle {
      position: absolute;
      left: 30px;
      top: 30px;
      z-index: 10;
  }


</style>
