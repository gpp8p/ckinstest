<template>
    <span class="lline">
        <span class="gitem">Label</span><span class="gitem">Description</span><span class="gitem">Height</span><span class="gitem">Width</span>
    </span>
</template>

<script>
  export default {
    name: "layoutListHeader"
  };
</script>

<style scoped>
    .lline {
        display: grid;
        grid-template-columns: 20% 60% 10% 10%;
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