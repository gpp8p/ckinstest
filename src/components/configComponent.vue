<template>

        <div id="drg" class="redClass" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
                <div class="configComponentHeader">
                        <span class="headingText">{{configElement[0].label}}</span>
                </div>
                <div class="configComponentBody">
                        <br/>
                        <configElement v-for="(configElement, index) in this.configurationElements[configurationLine].configurationElements"
                                       :currentValues="currentValues"
                                       :configElement="configElement"
                                       :key="index"
                                       :fieldNumber="index"
                                       :onePage="onePage"
                                       @configSelected="configSelected"  ></configElement>

                </div>
                <div class="linkFooter">
                        <next-cancel-link :show-next="this.next" :show-prev="this.prev" :show-save="this.save" @buttonClick="bumpLine" ></next-cancel-link>
                </div>
        </div>

</template>

<script>
        import configElement from "../components/configElement.vue";
        import nextCancelLink from "../components/nextCancelLink.vue";

        export default {
        name: "configComponent",
        components: {configElement, nextCancelLink},
        data(){
            return {
               mouseIsDown:false,
               firstMouseX:0,
               firstMouseY:0,
               lastMouseX:0,
               lastMouseY:0,

               expandedElementNow: 'null',
               closeExpanderFunction: null,
               configurationLine: 0,
               activeInputField:0,
               inputElementsOnPage:0,
               configurationElements:this.configElement,
               configurationCurrentValues:this.currentValues,
               next: false,
               prev: false,
               save: false,
               onePage: false

            }
        },
        props: {
                    configElement: {
                            type: Array,
                            required: true
                    },
                    currentValues:{
                            type: Object,
                            required:true
                    },
         },
        mounted(){
                console.log('configComponent mounted');
                if(this.configElement.length>1){
                        this.onePage=false;
                }else{
                        this.onePage=true;
                }
                for(var e=0;e<this.configurationElements[this.configurationLine].configurationElements.length;e++){
                     if(this.configurationElements[this.configurationLine].configurationElements[e].type=='input'){
                             this.inputElementsOnPage++;
                     }
                }
                if(this.onePage){
                        this.next=false;
                        this.prev=false;
                        this.save=true;
                }else{
                        this.next=true;
                        this.prev=false;
                        this.save=false;
                }
                this.activeInputField=0;
        },

        methods: {
            handleDragStart(evt){
              this.firstMouseX = evt.screenX;
              this.firstMouseY = evt.screenY;
              this.$emit('startDrag', [this.firstMouseX,this.firstMouseY]);
            },
            handleDragEnd(evt){
                    // eslint-disable-next-line no-debugger
        //               debugger;
               this.lastMouseX = evt.screenX;
               this.lastMouseY = evt.screenY;
               this.$emit('newLocation',[this.lastMouseX, this.lastMouseY]);
                    // eslint-disable-next-line no-console
               console.log(evt);
            },
            bumpField(){
               this.activeInputField++;
            },
            configSelected(msg){
                        // eslint-disable-next-line no-debugger
                        debugger;
                        // eslint-disable-next-line no-console
                        console.log('configSelected');
                        // eslint-disable-next-line no-console
                        console.log(msg);
                        if (msg[1] == 'activated'){
                                //open expander function
                                msg[2](msg[0]);
                                this.closeExpanderFunction = msg[3];
                        }else if(msg[1]== 'deactivated'){
                                msg[3](msg[0]);
                                //          this.closeExpanderFunction();
                        }
                        if(typeof msg[4] != 'undefined'){
                                if(msg[4]==true){
                                        if(this.activeInputField<this.configurationElements[this.configurationLine].configurationElements.length){
                                                this.activeInputField++;
                                        }else{
                                                this.activeInputField=0;
                                                this.bumpLine(['next']);
                                        }

                            /*
                                        if(this.onePage){
                                                this.activeInputField++;
                                        }else{
                                                this.bumpLine(['next']);
                                        }

                             */

                                }
                        }
                        this.$emit('configSelected',[msg[0],msg[1],this.openExpander, this.closeExpander ]);
             },

             bumpLine(msg){
  //      debugger;
                if(msg[0]=='cancel'){
                        this.$emit('configSelected',[msg[0]]);
                }
                if(msg[0]=='save'){
                        this.$emit('configSelected',[msg[0]]);
                }
//                if(this.configurationLine<(this.configurationElements.length-1)){
                        switch(msg[0]){
                                 case 'next':
                                        this.configurationLine++;
                                         this.inputElementsOnPage=0;
                                         for(var e=0;e<this.configurationElements[this.configurationLine].configurationElements.length;e++){
                                                 if(this.configurationElements[this.configurationLine].configurationElements[e].type=='input'){
                                                         this.inputElementsOnPage++;
                                                 }
                                         }

                                         if(this.configurationLine==(this.configurationElements.length-1)){
                                                 this.next=false;
                                                 this.save=true;
                                         }
                                         if(this.configurationLine>0){
                                                 this.prev=true;
                                         }
                                        break;
                                case 'previous':
                                        this.configurationLine--;
                                        this.inputElementsOnPage=0;
                                        for(e=0;e<this.configurationElements[this.configurationLine].configurationElements.length;e++){
                                                if(this.configurationElements[this.configurationLine].configurationElements[e].type=='input'){
                                                        this.inputElementsOnPage++;
                                                }
                                        }

                                        if(this.configurationLine==0){
                                                this.prev=false;
                                        }
                                        if(this.configurationLine<(this.configurationElements.length-1)){
                                                this.next=true;
                                                this.save=false;
                                        }
                                        break;
                        }

//                }
             }

        }
    }
</script>


<style scoped>
    .redClass {
        height:250px;
        width:500px;
        background-color: #ab97ff;
        border: 2px solid blue;
        border-radius: 8px;
        box-shadow: 10px 10px 5px grey;
    }
        .configComponentHeader{
                height:10%;
                background-color: #fff722;
                border-top-left-radius: 8px;
                border-top-right-radius: 8px;
                text-align: center;
                color: blue;
                font-family: Geneva;
                font-size: 12px;
                font-style: normal;
                font-weight: bold;
        }
        .headingText{
                margin-top: 5px;
        }
        .configComponentBody {
                height: 80%;
                margin-left: 10px;
                margin-right: 10px;
        }
        .linkFooter {
                height: 10%;
                margin-left: 10px;
                margin-right: 10px;
        }

</style>