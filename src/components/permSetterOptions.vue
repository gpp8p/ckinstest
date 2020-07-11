<template>
    <div class="dialogComponentFooter" >
        <span class="linkStyle" @click="done">
           Done
        </span>
        <span v-if="this.permSetterViews.showPermsShow==true" @click="perms" class="linkStyle">
            Permissions
        </span >
        <span v-if="this.permSetterViews.addMemberShow==true" @click="addMember" class="linkStyle">
            Add Member
        </span >
        <span v-if="this.permSetterViews.showMemberInfoShow==true" @click="showMemberInfo" class="linkStyle">
            Group Members
        </span>
        <span v-if="this.permSetterViews.addNewGroupShow==true" @click="showMemberInfo" class="linkStyle">
            Add Group
        </span>
    </div>
</template>

<script>
    export default {
        name: "permSetterOptions",
        props:{
            permSetterViews: {
                type: Object,
                required: true
            }
        },
        methods: {
            done(){
                this.$emit('permOptionClicked', 'Done');
            },
            perms(){
                this.$emit('permOptionClicked', 'Permissions');
            },
            addMember(){
                this.$emit('permOptionClicked', 'Add Member');
            },
            showMemberInfo(){
                this.$emit('permOptionClicked', 'Group Members');
            }
        }
    }
</script>

<style scoped>
    .dialogComponentFooter {
        display:flex;
        justify-content: space-around;
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
