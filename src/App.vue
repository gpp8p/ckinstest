<template>
    <span class="layoutScreen">
      <section class="navbar">
        <div v-if="this.navBarView==this.VIEW_TOP_MENU">
            <span class="layoutMenu"><span class="layoutMenuItem" @click="createLayout">New Layout</span><span class="layoutMenuItem">User Administration</span></span>
        </div>
      </section>
      <section class="content">
        <span v-if="this.contentView==this.VIEW_TEST_CKEDITOR" class="editor">
          <ckeditor-component></ckeditor-component>
        </span>
        <div v-if="this.contentView==this.VIEW_LAYOUT_LIST">
            <layout-list></layout-list>
        </div>


      </section>
    </span>
</template>

<script>
  import CkeditorComponent from './components/ckeditorComponent.vue'
  import CKEditor from '@ckeditor/ckeditor5-vue';
  import Vue from 'vue';
  import layoutList from './components/LayoutList.vue';


  Vue.use( CKEditor );

  export default {
    name: 'app',
    components: { layoutList, CkeditorComponent },
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
        contentView: this.VIEW_TOP_MENU,
        navBarView: this.VIEW_TOP_MENU,
        showCkTest: false,
        allLayouts: []
      }
    },
    methods:{
        layoutSelected(){
            console.log('layoutSelected');
        },
        createLayout(){
            console.log('createLayout summoned');
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
