<template>
    <span class="lline" v-on:click="this.layoutSelected"><span class="gitem">{{menu_label}}</span><span class="gitem">{{description}}</span></span>
</template>

<script>
  /* eslint-disable no-debugger */

  export default {
    name: "LayoutListLine",
    props:
      {
        id: {
          type: String,
          required: true
        },
        menu_label: {
          type: String,
          required: true
        },
        description: {
          type: String,
          required: true
        },
        height: {
          type: String,
          required: true
        },
        width: {
          type: String,
          required: true
        }
      },
// eslint-disable-next-line no-debugger
    methods:{
      layoutSelected(){
        this.$emit('layoutSelected', [this.id])
      }
    }
  };
</script>

<style scoped>
    .lline {
        display: grid;
        grid-template-columns: 20% 60%;
        font-size: medium;
        font-family: Arial;

    }
    .lline:hover {
        background-color: burlywood;
    }
    .gitem {
        text-align: left;
        margin-left: 5px;
    }


</style>