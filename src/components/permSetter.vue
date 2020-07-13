<template>
    <div class="dialogComponent" ref="drg"  draggable="true"  @dragstart="handleDragStart" @dragend="handleDragEnd" >
        <div class="dialogComponentHeader">
            <span class="headingText">Who Can Access This Space ?</span>
        </div>
        <div class="dialogComponentBody">
            <perm-list :layoutId="layoutId" ref="permlist" @cancelClicked="cancelClicked" @showGroupMembers="showGroupMembers" @showPerms="showPerms" @showNewGroup="showNewGroup" @groupMembersLoaded="groupMembersLoaded" ></perm-list>
        </div>
        <div>
            <perm-setter-options :permSetterViews="permSetterViews" @permOptionClicked="permOptionClicked"></perm-setter-options>
        </div>

    </div>
</template>

<script>
    import PermList from "./permList";
    import permSetterOptions from "./permSetterOptions.vue";
    export default {
        name: "permSetter",
        components: {PermList, permSetterOptions},
        watch:{
           selectedGroup: function(){
              console.log('selected group hasw changed');
//              this.addMemberShow=true;
           }
        },
        props: {
          layoutId:{
              type: Number,
              required: true
          }
        },
        data(){
            return {
                pview:0,
                PERMS:0,
                GROUP_INFO:1,
                NEW_GROUP:2,
                ADD_MEMBER_TO_GROUP:3,
                ADD_NEW_MEMBER:4,
                ORGANIZATION_GROUPS:6,
                selectedGroup:0,
                addMemberShow: false,
                showPermsShow:true,
                showGroupMembersShow:false,
                permSetterViews:{
                    showPermsShow: false,
                    showDoneShow:true,
                    showMemberInfoShow:false,
                    showSelectNewGroupShow:true,
                    addNewGroupShow:true,
                    addMemberShow:false
                }


            }
        },

        methods:{
            cancelClicked(){
 //               debugger;
                this.$emit('configSelected',['permSetterCanceled']);
            },
            showGroupMembers(msg){
                console.log('showGroupMembers',msg);
                this.permSetterViews.showMemberInfoShow=false;
                this.permSetterViews.showPermsShow=true;
                this.permSetterViews.addNewGroupShow=false;
                this.permSetterViews.addMemberShow=true;
//                  this.$emit('permSetterOptionChange', 'showGroupMembers' );
//                this.view = this.GROUP_INFO;
//                this.showGroupMembersShow=true;
//                this.selectedGroup=msg[0];
            },
            groupMembersLoaded(){
                console.log('group members loaded');
//                this.addMemberShow=true;
            },
            permOptionClicked(msg){
                debugger;
                switch(msg){
                    case 'Done':
                        this.cancelClicked();
                        break;
                    case 'Permissions':
                        this.$refs.permlist. setView(this.PERMS);
//                        this.pview = this.PERMS;
                        this.permSetterViews.showPermsShow=false;
                        this.permSetterViews.addMemberShow=false;
                        this.permSetterViews.addNewGroupShow=true;
                        break;
                    case 'Add Group':
                        this.$refs.permlist.setView(this.ORGANIZATION_GROUPS);
                        this.permSetterViews.showPermsShow=true;
                        this.permSetterViews.addNewGroupShow=false;
                        this.$refs.permlist.showOrganizationGroups(this.$store.getters.getOrgId);
                        break;
                }
            },
            showNewGroup(){

            },
            showAllUsers(){
                this.view=this.ADD_MEMBER_TO_GROUP;
                this.$refs.permList.showAllUsers();
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


</style>
