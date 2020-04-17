<template>
     <a href="#" :class="{ highlight: this.highlight, linkStyle: !this.highlight}" v-on:click="buttonClicked" >{{ buttonLabel }}</a>
</template>

<script>
export default {
  name: "MyButton",
  props: {
    buttonLabel: {
      type: String,
      required: true
    },
    highlight: {
         type: Boolean,
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
          color: blue;
          font-family: Geneva;
          font-size: 12px;
          font-style: normal;
          font-weight: bold;

     }
     .highlight {
          background-color: #feff06;
          margin-left: 10px;
          margin-right: 10px;
          color: blue;
          font-family: Geneva;
          font-size: 12px;
          font-style: normal;
          font-weight: bold;

     }
     /* unvisited link */
     a:hover {
          color: hotpink;
     }

     .accent {
          background-color: #fff722;
          color: blue;
     }

     .normal {
          background-color: #ab97ff;
          color: blue;
     }


     a:active {
          color: blue;
     }


</style>
