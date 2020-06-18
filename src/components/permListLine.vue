<template>
    <span class="plline">
        <perm-list-group :groupId="groupId" :groupDescription="groupDescription"></perm-list-group>
        <perm-list-perm :perm-type="this.VIEW_TYPE" :permValue="viewValue" @permChanged="permChanged"></perm-list-perm>
        <perm-list-perm :perm-type="this.AUTHOR_TYPE" :permValue="authorValue" @permChanged="permChanged"></perm-list-perm>
        <perm-list-perm :perm-type="this.ADMIN_TYPE" :permValue="adminValue" @permChanged="permChanged"></perm-list-perm>
    </span>
</template>

<script>
    import permListPerm from '../components/permListPerm.vue';
    import permListGroup from '../components/permListGroup.vue';
    export default {
        name: "permListLine",
        props:{
          groupId:{
              type: Number,
              required: true
          },
          groupDescription:{
              type: String,
              required: true
          },
          viewValue:{
              type: Number,
              required: true
          },
          authorValue:{
              type: Number,
              required: true
          },
          adminValue:{
              type: Number,
              required:true
          },
          opt1Value:{
              type: Number,
              required: true
          },
          opt2Value:{
              type: Number,
              required: true
          },
          opt3Value:{
              type: Number,
              required: true
          },
        },
        components: {permListPerm, permListGroup},
        data(){
            return {
                VIEW_TYPE: 'view',
                AUTHOR_TYPE: 'author',
                ADMIN_TYPE: 'admin',
                OPT1_TYPE: 'opt1',
                OPT2_TYPE: 'opt2',
                OPT3_TYPE: 'opt3'
            }
        }
    }
</script>

<style scoped>

    .plline {
        width: 100%;
        display: grid;
        grid-template-columns: 50% 13% 13% 13% 13%;
        font-size: medium;
        font-family: Arial;

    }

</style>
