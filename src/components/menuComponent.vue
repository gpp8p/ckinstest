<template>
    <span class="layoutMenu">
        <menu-item v-for="(label, index) in items"
                   :itemLabel="label"
                   :key="index"
                   @menuSelection="itemSelected"
        ></menu-item>
        <login-component @tokenInstalled="tokenInstalled" @userLogged="userLogged"></login-component>
    </span>
</template>

<script>
    import menuItem from '../components/menuItem.vue';
    import loginComponent from "../components/loginComponent.vue";
    export default {
        name: "menuComponent",
        components: { menuItem, loginComponent },
        props:{
            items:{
                type: Array,
                required: true
            }
        },
        methods:{
            itemSelected(msg){
                this.$emit('menuSelection', [msg[0]]);
            },
            tokenInstalled(msg){
//                debugger;
                this.$emit('tokenInstalled', [msg]);
            },
            userLogged(msg){
//                debugger;
                this.$emit('userLogged', [msg]);
            }
        }
    }
</script>

<style scoped>
    .layoutMenu {
        display: flex;
        justify-content: space-evenly;
        height: 100%;
        align-items: baseline;
        margin-top: 6px;
    }


</style>
