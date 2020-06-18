<template>
    <span class="plline">
        <span>User or Group</span>
        <span>View</span>
        <span>Author</span>
        <span>Administer</span>
    </span>
</template>

<script>
    export default {
        name: "permListHeader"
    }
</script>

<style scoped>
    .plline {
        margin-top: 2%;
        width: 100%;
        display: grid;
        grid-template-columns: 50% 13% 13% 13% 13%;
        font-size: medium;
        font-family: Arial;
        background-color: darkgreen;
        color: white;


    }
</style>
