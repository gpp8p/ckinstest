<template>
    <span>
       <orgListHeader></orgListHeader>
                <orgListLine v-for="(org, index) in allOrgs"
                                :key="index"
                                :id="org.id.toString()"
                                :description="org.description"
                                :org_label="org.org_label"
                                @orgSelected="orgSelected"
                >
                </orgListLine>

    </span>
</template>

<script>
    import axios from "axios";
    import orgListHeader from "../components/orgListHeader.vue";
    import orgListLine from "../components/orgListLine.vue";
    export default {
        name: "orgList",
        components: {orgListHeader, orgListLine},
        mounted: function() {
            console.log('mounted runs in  orgList');
//            axios.get('http://localhost:8000//layoutList')
            axios.get('http://localhost:8000/api/shan/orgList?XDEBUG_SESSION_START=19181')
                .then(response => {
// eslint-disable-next-line no-debugger
                    // JSON responses are automatically parsed.
                    this.allOrgs = response.data;
                    console.log('successful return from orgList');
                })
                .catch(e => {
                    console.log(e);
                    this.errors.push(e);
                });
        },
        data(){
            return{
                allOrgs:[]
            }
        },
        methods:{
            orgSelected(msg){
                this.$emit('orgSelected',[msg]);
                console.log('orgSelected summoned:'+msg);
            }
        }

    }
</script>

<style scoped>

</style>
