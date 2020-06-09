<template>
    <span class="lline">
        <span class="gitem">Label</span><span class="gitem">Description</span>
    </span>
</template>

<script>
  export default {
    name: "layoutListHeader"
  };
</script>

<style scoped>
    .lline {
        margin-top: 2%;
        display: grid;
        grid-template-columns: 20% 60%;
        font-size: medium;
        font-family: Arial;
        background-color: darkgreen;
        color: white;
    }
    .gitem {
        text-align: left;
        margin-left: 5px;
    }

</style>
