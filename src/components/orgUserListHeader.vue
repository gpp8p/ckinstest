<template>
    <span class="lline">
        <span class="gitem">Email</span><span class="gitem">Name</span>
    </span>

</template>

<script>
    export default {
        name: "orgUserListHeader"
    }
</script>

<style scoped>
    .lline {
        display: grid;
        grid-template-columns: 30% 70%;
        font-size: medium;
        font-family: Arial;
        align-items: left;
        background-color: darkgreen;
        color: white;
    }
    .gitem {
        text-align: left;
        margin-left: 5px;
    }
</style>
