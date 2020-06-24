<template>
    <span>
       <layoutListHeader></layoutListHeader>
                <LayoutListLine v-for="(layout, index) in allLayouts"
                                :key="index"
                                :id="layout.id.toString()"
                                :description="layout.description"
                                :menu_label="layout.menu_label"
                                :height="layout.height.toString()"
                                :width="layout.width.toString()"
                                @layoutSelected="layoutSelected"
                >
                </LayoutListLine>

    </span>
</template>

<script>
    import axios from "axios";
    //  import LayoutListLine from './components/LayoutListLine.vue';
    import layoutListHeader from '../components/layoutListHeader.vue';
    import LayoutListLine from '../components/LayoutListLine.vue';

    export default {
        name: "LayoutList",
        components: { LayoutListLine, layoutListHeader},
        mounted: function() {
            console.log('mounted runs in layout list');
//            axios.get('http://localhost:8000//layoutList')
            this.reloadLayouts();
        },
        data(){
            return{
                allLayouts:[]
            }
        },
        props: {
            orgId:{
                orgId: Number,
                required: true
            }
        },
        watch: {
          orgId: function(){
              this.reloadLayouts();
          }
        },
        methods:{
            layoutSelected(msg){
                this.$emit('layoutSelected',[msg[0]]);
                console.log('layoutSelected summoned'+msg);
            },
            reloadLayouts(){
                if(this.orgId>0){

                    axios.get('http://localhost:8000/api/shan/orgLayouts?XDEBUG_SESSION_START=11354', {
                        params:{
                            orgId:this.orgId,
                        }
                    })
                        .then(response => {
// eslint-disable-next-line no-debugger
                            // JSON responses are automatically parsed.
                            this.allLayouts = response.data;
                            console.log('successful return from layoutList');
                        })
                        .catch(e => {
                            console.log(e);
                            this.errors.push(e);
                        });
                }else{
                    axios.get('http://localhost:8000/api/shan/layoutList')
                        .then(response => {
// eslint-disable-next-line no-debugger
                            // JSON responses are automatically parsed.
                            this.allLayouts = response.data;
                            console.log('successful return from layoutList');
                        })
                        .catch(e => {
                            console.log(e);
                            this.errors.push(e);
                        });
                }
            }
        }
    }
</script>

<style scoped>


</style>
