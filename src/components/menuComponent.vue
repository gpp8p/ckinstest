<template>
    <span class="layoutMenu">
        <menu-item v-for="(label, index) in items"
                   :itemLabel="label"
                   :key="index"
                   @menuSelection="itemSelected"
        ></menu-item>
        <login-component></login-component>
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
