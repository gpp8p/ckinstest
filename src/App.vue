<template>
    <span class="layoutScreen">
        <div v-if="this.floatingView==this.VIEW_FLOATING_CONFIG" id="infoi" v-bind:style='styleObject' >
            <config-component v-if="this.draggedComponent=='configComponent'"
                          @newLocation="this.setNewLocation"
                          @startDrag="this.startDrag"
                          :currentValues=this.cardCurrentConfigurationValues
                          :configElement=this.cardConfigurationElements
                          :onePage=this.onePageStatus
                          @configSelected="configSelected" >
            ></config-component>
        </div>

      <section class="navbar">
        <div v-if="this.navBarView==this.VIEW_TOP_MENU">
            <menu-component :items="this.topMenuItems" @menuSelection="menuSelection"></menu-component>
        </div>
        <div v-if="this.navBarView==this.VIEW_GRID_MENU">
            <menu-component :items="this.editMenuItems" @menuSelection="menuSelection"></menu-component>
        </div>

      </section>
      <section class="content">
        <span v-if="this.contentView==this.VIEW_TEST_CKEDITOR" class="editor">
          <ckeditor-component></ckeditor-component>
        </span>
        <div v-if="this.contentView==this.VIEW_LAYOUT_LIST">
            <layout-list @layoutSelected="layoutSelected"></layout-list>
        </div>
        <layout  :layoutId="selectedLayoutId" :layoutCmd="layoutCmd" ref="editGrid" @cardClick="cardClick" @storeValue="cellClicked" @configurationHasBeenSaved="configurationHasBeenSaved" @cardDataLoaded="cardDataLoaded" @linkHelperRequested="linkHelperRequested"></layout>
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


  Vue.use( CKEditor );

  export default {
    name: 'app',
    components: {Layout, layoutList, CkeditorComponent, menuComponent, configComponent },
    mounted: function() {
          this.navBarView = this.VIEW_TOP_MENU;
          this.contentView = this.VIEW_LAYOUT_LIST;
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
            contentView: this.VIEW_TOP_MENU,
            navBarView: this.VIEW_TOP_MENU,
            floatingView: this.VIEW_TOP_MENU,
            showCkTest: false,
            allLayouts: [],
            topMenuItems: ['New Layout', 'UserAdministration'],
            editMenuItems: ['Edit', 'Display', 'Layout List'],
            selectedLayoutId: '',
            layoutCmd: '',



            componentLeft: 0,
            componentTop: 0,
            styleObject: {
                left: '200px',
                top: '300px',
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
            onePageStatus:false,




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
        }
    },
    methods:{
        layoutSelected(msg){
            console.log('layoutSelected');
            this.navBarView = this.VIEW_GRID_MENU;
            this.contentView = this.VIEW_GRID_MENU;
            this.layoutCmd='show';
            this.selectedLayoutId=msg[0];
        },
        configSelected(msg){
            switch(msg[0]){
                case 'cancel':
                    this.navBarView = this.VIEW_TOP_MENU;
                    this.contentView = this.VIEW_LAYOUT_LIST;
                    this.draggedComponent="";
                    this.layoutCmd = 'hide';
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
            switch(msg[0]){
                case 'Layout List':
                    this.navBarView = this.VIEW_TOP_MENU;
                    this.contentView = this.VIEW_LAYOUT_LIST;
                    this.layoutCmd='hide';
                    break;
                case 'New Layout':

//                    this.floatingView = this.VIEW_FLOATING_CONFIG;
//                    this.cardCurrentConfigurationValues={};
//                    this.cardConfigurationElements = this.newLayoutConfig;
//                    this.onePageStatus=true;
//                    this.draggedComponent='newLayout';
//                    this.draggedComponent='configComponent';
                    this.layoutCmd = 'new';
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
        cardClick(msg){
            debugger;
            console.log(msg);

            this.configCard=true;
            this.cardTypeBeingConfigured = msg[2]
            this.instancePositionBeingConfigured = msg[1];
            this.screenElementBeingConfigured = msg[4];
            this.cardDataFunction = msg[3];
            this.cardConfigurationElements=msg[4];
            this.cardCurrentConfigurationValues=msg[5];
            this.floatingView = this.VIEW_FLOATING_CONFIG;
            this.draggedComponent = 'configComponent';
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


</style>
