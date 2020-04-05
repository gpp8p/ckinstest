<template>
     <a href="#" class="linkStyle" v-on:click="buttonClicked" >{{ buttonLabel }}</a>
</template>

<script>
export default {
  name: "MyButton",
  props: {
    buttonLabel: {
      type: String,
      required: true
    }
  },
  methods: {
    buttonClicked: function() {
      this.$emit('myButtonClicked', [this.buttonLabel])
    }
  }
};
</script>

<style scoped>
     .linkStyle {
          margin-left: 10px;
          margin-right: 10px;
          color: #0537aa;
     }
     /* unvisited link */
     a:hover {
          color: hotpink;
     }


     a:active {
          color: blue;
     }


</style>
