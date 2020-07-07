<template>
    <div class="dialogComponent" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <span class="headingText">Who Can Access This Space ?</span>
        </div>
        <div class="dialogComponentBody">
            <perm-list :permView="this.permView" :layoutId="this.layoutId" :selectedId="groupId" @groupClicked="groupClicked"></perm-list>
            <group-member-list v-if="this.permView==this.GROUP_INFO" ref="memberList" v-bind:groupId="this.groupId" v-bind:permView="this.permView" ></group-member-list>
        </div>
        <div class="dialogComponentFooter">
            <a href="#" class="linkStyle" v-on:click="cancelClicked" >Done</a>
            <a href="#" class="linkStyle" v-on:click="showPerms">Permissions</a>
            <a href="#" v-if="addMemberShow" class="linkStyle" v-on:click="showAllUsers">Add Member</a>
        </div>
    </div>
</template>

<script>
    import PermList from "./permList2.vue"
    import GroupMemberList from "./groupMemberList.vue";
    export default {
        name: "permSetter2",
        components:{PermList, GroupMemberList},
        mounted(){
          this.permView=this.PERMS;
        },
        props:{
            layoutId:{
                type: Number,
                required: true
            }
        },
        methods:{
            groupClicked(msg){
 //               debugger;
                console.log('groupClicked', msg[0]);
//                this.groupId=msg[0];
                this.groupId=msg[0];
                debugger;
                this.permView=this.GROUP_INFO;
//                debugger;
//                this.$refs.memberList.loadMembers(this.groupId);
            }
        },
        data(){
            return {
                permView:0,
                PERMS:0,
                GROUP_INFO:1,
                groupId:45,
                selectedGroupId:0
            }
        }
    }
</script>

<style scoped>
    .dialogComponent {
        height:250px;
        width:500px;
        background-color: #ab97ff;
        border: 2px solid blue;
        border-radius: 8px;
        box-shadow: 10px 10px 5px grey;
    }
    .dialogComponentHeader {
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
    .dialogComponentBody {
        height: 72%;
        margin-left: 10px;
        margin-right: 10px;
        display: grid;
        grid-template-columns: 30% 70%;
        grid-template-rows: 15% 15% 15% 15%


    }
    .dialogComponentFooter {
        height: 10%;
        margin-left: 10px;
        margin-right: 10px;
    }
    .linkStyle{
        font-family: Arial;
        font-size: medium;
        color: #0a3aff;
    }

</style>
