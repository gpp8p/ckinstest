<template>
    <span v-if="status==this.EXISTING_LAYOUTS">
       <layoutLinksHeader></layoutLinksHeader>
                <LayoutLinksLine v-for="(layout, index) in allLayouts"
                                :key="index"
                                :id="layout.id.toString()"
                                :description="layout.description"
                                :menu_label="layout.menu_label"
                                :height="layout.height.toString()"
                                :width="layout.width.toString()"
                                @layoutSelected="layoutSelected"
                >
                </LayoutLinksLine>

    </span>
</template>

<script>
//    import axios from "axios";
    //  import LayoutListLine from './components/LayoutListLine.vue';
    import layoutLinksHeader from '../components/layoutLinksHeader.vue';
    import LayoutLinksLine from '../components/LayoutLinksLine.vue';
import axios from "axios";

    export default {
        name: "LayoutLinks",
        components: { LayoutLinksLine, layoutLinksHeader},
        mounted: function(){
          console.log('mounted runs in layoutLinks');
          console.log('orgId - according vuex:', this.$store.getters.getOrgId );
          debugger;
            axios.get('http://localhost:8000/api/shan/viewableLayouts', {
                params:{
                    orgId:this.$store.getters.getOrgId
                }
            })
                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    this.allLayouts = response.data;
                })
                .catch(e => {
                    this.errors.push(e);
                    console.log('viewableLayouts failed');
                });
        },
        data(){
            return{
                allLayouts:[],
                EXISTING_LAYOUTS:0,
                NEW_LAYOUT:1,
            }
        },
        props:{
            status:{
                type: Number,
                required:true
            }
        },
        methods:{
            layoutSelected(msg){
                this.$emit('layoutSelected',[msg[0]]);
                console.log('layoutSelected summoned'+msg);
//                debugger;
            }
        }
    }
</script>

<style scoped>
.layoutLinkList {
    width: 100%;
}

</style>
