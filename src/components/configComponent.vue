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
                                       @configSelected="selectionHandler_flexConfig2"  ></configElement>
                        <next-cancel-link :currentStatus="this.configurationLine" @buttonClick="bumpLine" ></next-cancel-link>
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
               configurationElements:this.configElement,
               configurationCurrentValues:this.currentValues

            }
        },
        props: {
                    configElement: {
                            type: Object,
                            required: true
                    },
                    currentValues:{
                            type: Object,
                            required:true
                    },
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
                selectionHandler_flexConfig2(msg){
                        // eslint-disable-next-line no-debugger
                        debugger;
                        // eslint-disable-next-line no-console
                        console.log('selectionHandler');
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
                                        this.bumpLine(['next']);
                                }
                        }
                        this.$emit('configSelected',[msg[0],msg[1],this.openExpander, this.closeExpander ]);
                },

                bumpLine(msg){
//        debugger;
                        if(this.configurationLine<this.configurationElements.length){
                                switch(msg[0]){
                                        case 'cancel':
                                                this.$emit('configSelected',[msg[0]]);
                                                break;
                                        case 'next':
                                                this.configurationLine++;
                                                break;
                                        case 'previous':
                                                this.configurationLine--;
                                                break;
                                }
                        }
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
                height: 90%;
                margin-left: 10px;
                margin-right: 10px;
        }

</style>