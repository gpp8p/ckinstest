<template>
    <span >
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
            axios.get('http://localhost:8000//layoutList')
                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    this.allLayouts = response.data;
                })
                .catch(e => {
                    this.errors.push(e);
                });
        },
        data(){
            return{
                allLayouts:[]
            }
        },
        methods:{
            layoutSelected(msg){
                this.$emit('layoutSelected',[msg[0]]);
                console.log('layoutSelected summoned'+msg);
            }
        }
    }
</script>

<style scoped>
.layoutLinkList {
    width: 100%;
}

</style>